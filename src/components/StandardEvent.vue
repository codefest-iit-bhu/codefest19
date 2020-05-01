<template>
  <div :class="[$style.event, $style[$mq], eventActiveClass]">
    <div :class="eventCellClass" @click="navigateToDetails">
      <div :class="$style.whiteTitle">
        <h3>{{ event.title }}</h3>
      </div>
      <div
        :class="$style.cell"
        :style="eventWidthStyle"
        @mouseenter="toggleOpen"
        @mouseleave="toggleOpen"
      >
        <canvas
          :class="$style.normalCanvas"
          ref="initialCanvas"
          :style="canvasOpacityStyle(true)"
          v-if="!isMinimal"
        />
        <canvas
          :class="$style.glitchedCanvas"
          ref="finalCanvas"
          :style="canvasOpacityStyle(false)"
        />
        <div :class="$style.txt">
          <p :class="$style.summary">
            <span ref="eventSummary"></span>
            <span :class="$style.blink">|</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { TypingAnim, getRandom, isMinimal } from "@js/utils";

export default {
  data() {
    return {
      shouldOpen: false,
      openProgress: 0,
      maxWidth: 0,
      minSize: 0,
    };
  },
  props: {
    event: {
      required: true,
      type: Object,
    },
    id: {
      required: true,
      type: Number,
    },
    keepOpen: {
      required: false,
      type: Boolean,
      default: false,
    },
  },
  methods: {
    animateGlitchOpacity(canvas, duration, isAppearIn) {
      if (!canvas) return;
      var that = this;
      var image = this.image;
      var verticalSlices = Math.round(image.height / 20);
      var maxHorizOffset = 30;

      var start = new Date().getTime();
      var context = canvas.getContext("2d");

      var step = function() {
        var timestamp = new Date().getTime();
        var progress = Math.min((timestamp - start) / duration, 1);
        canvas.style.opacity = isAppearIn ? progress : 1 - progress;

        context.clearRect(0, 0, canvas.width, canvas.height);

        for (var i = 0; i < verticalSlices; i++) {
          var horizOffset = getRandom(
            -Math.abs(maxHorizOffset),
            Math.abs(maxHorizOffset)
          );
          context.drawImage(
            image,
            0,
            i * verticalSlices,
            image.width,
            i * verticalSlices + verticalSlices,
            horizOffset,
            i * verticalSlices,
            image.width,
            i * verticalSlices + verticalSlices
          );
        }

        if (progress < 1) requestAnimationFrame(step);
        else {
          context.clearRect(0, 0, canvas.width, canvas.height);
          context.drawImage(image, 0, 0);
        }
      };
      return step();
    },
    glitch(isAppearIn) {
      if (!this.image) {
        return;
      }
      const { initialCanvas, finalCanvas } = this.$refs;
      this.animateGlitchOpacity(initialCanvas, 800, !isAppearIn);
      this.animateGlitchOpacity(finalCanvas, 800, isAppearIn);
    },
    canvasOpacityStyle(isNormalCanvas) {
      return {
        opacity: isNormalCanvas ? 1 - this.openProgress : this.openProgress,
      };
    },
    toggleEvent(shouldOpen) {
      TweenLite.to(this.$data, 0.8, { openProgress: shouldOpen ? 1 : 0 });
      this.glitch(shouldOpen);

      if (shouldOpen) this.animTyping.type();
      else this.animTyping.erase();
    },
    toggleOpen() {
      if (!this.isMinimal) this.shouldOpen = !this.shouldOpen;
    },
    resetState() {
      this.maxWidth = (this.isMinimal ? 1.0 : 0.7) * window.innerWidth;
      this.minSize = this.isMinimal ? 0.75 * 150 : 150;
    },
    navigateToDetails() {
      this.$router.push(`/events/${this.event.name}`);
    },
  },
  mounted() {
    function drawInCanvas(canvas, image) {
      if (!canvas) return;
      var context = canvas.getContext("2d");
      canvas.setAttribute("width", image.width);
      canvas.setAttribute("height", image.height);
      context.drawImage(image, 0, 0);
    }

    this.animTyping = new TypingAnim(
      this.$refs.eventSummary,
      this.event.summary
    );

    var img = new Image();
    img.src = this.event.icon;
    img.onload = () => {
      this.image = img;
      const { initialCanvas, finalCanvas } = this.$refs;
      drawInCanvas(initialCanvas, img);
      drawInCanvas(finalCanvas, img);
      if (this.keepOpen) {
        this.$nextTick(() => (this.shouldOpen = true));
      }
    };
  },
  beforeMount() {
    this.resetState();
  },
  computed: {
    eventCellClass() {
      return this.id % 2 == 0 ? this.$style.even : this.$style.odd;
    },
    eventActiveClass() {
      return this.isOpen || this.shouldOpen ? this.$style.visible : "";
    },
    eventWidthStyle() {
      const width = Math.max(this.minSize, this.openProgress * this.maxWidth);
      return {
        width: `${width}px`,
      };
    },
    eventTitleStyle() {
      const marginLeft = this.isMinimal ? undefined : "15px";
      const marginTop = this.isMinimal ? "15px" : undefined;
      return { marginLeft, marginTop };
    },
    isOpen() {
      return this.openProgress === 1;
    },
    isMinimal() {
      return isMinimal(this.$mq);
    },
  },
  watch: {
    shouldOpen: function(shouldOpen, oldVal) {
      if (oldVal == shouldOpen) return;
      this.toggleEvent(shouldOpen);
    },
    $mq: function() {
      this.resetState();
    },
  },
};
</script>

<style lang="stylus" module>
@require '~@styles/anims';

$cell-collapsed-size = 150px;

.event {
  --event-size: $cell-collapsed-size;
  height: var(--event-size);
  position: relative;
  margin: 0 20px 25px;
  cursor: pointer;
}

.whiteTitle {
  color: var(--text-color);
  font-family: 'Baloo Bhaina 2';

  h3 {
    $font-size: 32px;
  }
}

.cell {
  box-shadow: var(--icon-shadow);
  width: var(--event-size);
  height: 100%;
  background: $vermilion;
  border-radius: calc((var(--event-size) / 2));
}

.odd {
  height: 100%;
  display: flex;
  justify-content: flex-start;
  align-items: center;

  .normalCanvas {
    float: left;
  }

  .glitchedCanvas {
    float: right;
  }

  .txt {
    text-align: left;
    left: 10px;
  }

  .whiteTitle {
    order: 2;
    margin-left: 15px;
  }
}

.even {
  height: 100%;
  display: flex;
  justify-content: flex-end;
  align-items: center;

  .normalCanvas {
    float: right;
  }

  .glitchedCanvas {
    float: left;
  }

  .txt {
    text-align: right;
    left: -15px;
  }

  .whiteTitle {
    order: 0;
    margin-right: 15px;
  }
}

.txt {
  color: $black;
  $font-size: 20px;
  pointer-events: none;
  align-items: center;
  height: 100%;
  display: flex;
  transition: opacity 400ms;
  opacity: 0;
  overflow: visible;
  position: relative;

  .summary {
    font-family: 'Quicksand';
    padding: 2px 10px;
    text-align: justify;
    $font-size: 20vw;
    font-weight: 600;
    vertical-align: middle;
    color: $mystic;


  }


}

.normalCanvas, .glitchedCanvas {
  width: calc((var(--event-size) / 1));
  height: calc((var(--event-size) / 1));
  padding: 30px;
}

.glitchedCanvas {
  opacity: 0;
  pointer-events: none;
}

.blink {
  animation: blink 500ms infinite alternate;
}

.event {
  &.md {
    .txt {
      .summary{
        $font-size: 15vw;
      }
      
    }
  }

  &.visible {
    .txt {
      opacity: 1;
    }

    .cell {
      width: 70%;
    }

    &.md .cell, &.lg .cell, &.xl .cell {
      animation: timeline-border-green 1s ease-in-out infinite alternate;
    }
  }
}
</style>
