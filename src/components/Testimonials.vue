<template>
  <div :class="[$style.testimonials, $style[$mq]]">
    <div :class="$style.testimonial" v-for="(item, i) in testimonials" :key="i">
      <div :class="$style.avatar">
        <img :src="item.image" />
      </div>
      <div :class="$style.textbox">
        <div :class="$style.corner"></div>
        <div :class="$style.text">
          <h3>{{ item.name }}</h3>
          <p>{{ item.comment }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    testimonials: Array,
  },
};
</script>

<style module lang="stylus">
$ht = 120px;
$rd = 60px;

.testimonials {
  --item-height: $ht;
  --item-round: $rd;

  &.xs {
    --item-height: 2 * $ht;
    --item-round: $rd * 0.5;
  }

  &.sm {
    --item-height: 1.2 * $ht;
    --item-round: $rd * 0.66;
  }

  &.md, &.lg {
    --item-height: 1.5 * $ht;
  }

  .testimonial {
    width: 80%;
    margin-left: 10%;
    display: flex;
    flex-wrap: no-wrap;
    margin-bottom: 40px;
    margin-top: 40px;

    .avatar {
      order: 0;
      border-radius: var(--item-round);
      height: calc(var(--item-round) * 2);
      width: calc(var(--item-round) * 2);
      background: $mystic;
      padding: calc((var(--item-round) / 6));
      box-shadow: var(--icon-shadow);

      img {
        height: calc(var(--item-round) * (5/3));
        width: calc(var(--item-round) * (5/3));
        border-radius: 60px;
        object-fit: cover;
      }
    }

    .textbox {
      order: 1;
      display: flex;
      flex-flow: row;

      .corner {
        order: 2;
        min-width: var(--item-round);
        height: var(--item-round);
        background-repeat: no-repeat;
      }

      .text {
        order: 3;
        background: $vermilion;
        padding: 15px calc(var(--item-round) * 0.68);

        h3 {
          color: var(--background-color);
          margin: 0;
          font-family: 'Roboto Slab';

          ~/.xs ^[1..-1], ~/.sm ^[1..-1] {
            $font-size: 14px;
          }
        }

        p {
          color: var(--background-color);
          $font-size: 18px;
          font-weight: 700;

          ~/.xs ^[1..-1], ~/.sm ^[1..-1], ~/.md ^[1..-1] {
            $font-size: 12px;
          }
        }
      }
    }

    ~/.xs ^[1..-1] {
      width: 95%;
      margin-left: 2.5%;
    }
  }

  .testimonial:nth-child(odd) {
    flex-flow: row;

    .textbox {
      flex-flow: row;

      .corner {
        background-image: url('~@assets/Testimonial/corner-odd.svg');
      }

      .text {
        border-bottom-left-radius: var(--item-round);
        border-bottom-right-radius: calc(2 * var(--item-round));

        ~/.xs ^[1..-1], ~/.md ^[1..-1] {
          border-bottom-right-radius: var(--item-round);
          border-top-right-radius: var(--item-round);
        }
      }
    }
  }

  .testimonial:nth-child(even) {
    flex-flow: row-reverse;

    .textbox {
      flex-flow: row-reverse;
      text-align: right;

      .corner {
        background-image: url('~@assets/Testimonial/corner-even.svg');
      }

      .text {
        border-bottom-right-radius: var(--item-round);
        border-bottom-left-radius: calc(2 * var(--item-round));

        ~/.xs ^[1..-1], ~/.md ^[1..-1] {
          border-bottom-left-radius: var(--item-round);
          border-top-left-radius: var(--item-round);
        }
      }
    }
  }
}
</style>
