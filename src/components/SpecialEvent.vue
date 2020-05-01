<template>
  <div :class="[$style.hero, $style[$mq]]">
    <div>
      <router-link to="haxplore">
        <div :class="$style.logoContainer">
          <img :class="$style.logo" src="@assets/haxplore/logo-landing.svg" />
        </div>
      </router-link>
      <div :class="$style.title">
        <a>
          <router-link to="haxplore">
            <h1>HaXplore</h1>
          </router-link>
        </a>
        <p>
          Join the Codefest '20 team to bring out the absolute best ideas and
          implementations from some of the best developers in the country, while
          they join us for a 24 hours long hackathon under a single roof.
        </p>
      </div>
    </div>
    <div :class="$style.note">
      Note: People not registered on CodeFest are not eligible to receive any
      prizes.
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  mounted() {}
};
</script>

<style module lang="stylus">
@require '~@styles/anims';

.hero {
  position: relative;
  background-color: var(--background-color);
  min-height: 300px;
  height: 100%;
  width: 100%;
  overflow: hidden;
  padding: 10vh 16px;
  text-align: center;

  .logoContainer {
    width: 35vw;
    // min-width: 200px;
    max-width: 350px;
    border-radius: 100%;
    margin: 20px auto 40px;
    box-shadow: var(--box-shadow), inset 8px 8px 20px $crown-of-thorns;
    background-color: $vermilion;

    .logo {
      width: 90%;
      // height: 90%;
      padding: 15%;

      // ~/.xxl ^[1..-1] {
      //   padding: 80px;
      // }

      // ~/.xl ^[1..-1] {
      //   padding: 35px;
      // }

      // ~/.lg ^[1..-1] {
      //   padding: 30px;
      // }

      // ~/.md ^[1..-1] {
      //   padding: 15%;
      // }

      // ~/.xs ^[1..-1], ~/.sm ^[1..-1] {
      //   padding: 45px;
      // }
    }

    ~/.xs ^[1..-1], ~/.sm ^[1..-1] {
      width: 60vw;
      margin: 20px auto;
    }

    ~/.sm ^[1..-1] {
      width: 50vw;
      margin: 20px auto;
    }


  }

  .title {
    width: 100%;
    margin: 30px auto 0;
    text-align: center;

    a:hover {
      text-decoration: none;
    }

    h1 {
      font-family: 'Ubuntu';
      letter-spacing: 3px;
      $font-size: 48px;
      padding-top: 16px;
      padding: 20px;
      color: var(--text-color);
      box-shadow: var(--box-shadow);
      width: 360px;
      border-radius: 30px 0;
      margin: 0 auto;

      ~/.xs ^[1..-1], ~/.sm ^[1..-1] {
        $font-size: 24px;
        padding-top: 12px;
        padding: 10px;
        width: 250px;
      }
    }

    p {
      margin-top: 72px;
      font-family: 'Quicksand';
      font-weight: 500;
      $font-size: 26px;
    }

    ~/.xs ^[1..-1], ~/.sm ^[1..-1] {
      $font-size: 14px;

      p {
        $font-size: 20px;
      }
    }
  }
}

.note {
  font-family: 'Roboto Slab';
  font-weight: 700;
  text-align: center;
  $font-size: 24px;
}
</style>
