<template>
  <footer :class="$style.footer">
    <div :class="$style.logo">
      <div :class="$style.image">
        <img src="@assets/white-cf20-logo.svg" />
      </div>
      <p>Copyright © CodeFest'20</p>
      <p><a href="/privacy">Privacy Policy</a></p>
    </div>
    <i :class="$style.heart" />
    <span :class="$style.socialIcons">
      <a href="https://www.facebook.com/codefest/">
        <span class="fa-stack fa-lg">
          <!-- <i class="fa fa-circle-notch fa-stack-2x"></i> -->
          <i class="fab fa-facebook-f fa-stack-1x"></i>
        </span>
      </a>
      <a href="https://twitter.com/c0defest">
        <span class="fa-stack fa-lg">
          <!-- <i class="fa fa-circle-notch fa-stack-2x"></i> -->
          <i class="fab fa-twitter fa-stack-1x"></i>
        </span>
      </a>
      <a href="https://www.instagram.com/codefest.iitbhu">
        <span class="fa-stack fa-lg">
          <!-- <i class="fa fa-circle-notch fa-rotate fa-stack-2x"></i> -->
          <i class="fab fa-instagram fa-stack-1x"></i>
        </span>
      </a>
    </span>
  </footer>
</template>

<script>
export default {};
</script>

<style module lang="stylus">
.footer {
  $font-size: 10px;
  font-weight: 800;
  background: $mine-shaft;
  letter-spacing: 0.5px;
  text-transform: uppercase;
  font-family: 'Quicksand';
  border-top: 2px solid rgba(34, 40, 49, 0.3);
  color: rgba($white, 0.65);
  justify-content: center;
  align-items: center;
  display: flex;
  flex-flow: row;
  flex-wrap: wrap;
  margin-top: 20px;
  width: 100%;
  box-shadow: var(--appbar-shadow-color) 0 -1px 10px 3px;
  .logo {
    width: 100%;
    margin-top: 20px;

    .image {
      width: 160px;
      margin: 0 auto;

      img {
        width: 100%;
      }
    }

    p {
      text-align: center;
      color: $white;
    }
  }

  span {
    font-family: 'Quicksand';

    a {
      color: inherit;
      transition: color var(--transition-duration);
    }
  }

  a {
    &:hover {
      color: rgba(255, 255, 255, 1);
    }
  }

  .socialIcons {
    margin-left: 10px;
    margin-bottom: 5px;

    a {
      position: relative;
      width: 30px;
      height: 30px;
    }
  }
}

.heart {
  width: 60px;
  height: 60px;
  display: inline;
  background: transparent url('~@assets/twitter_heart.png') no-repeat;
  background-size: 2900%;
  margin: 0 0;

  &:hover {
    background-position: right;
    animation: moveHeart 800ms steps(28) forwards;
  }
}

@keyframes moveHeart {
  0% {
    background-position: left;
  }

  50% {
    background-position: right;
  }

  100% {
    background-position: right;
  }
}
</style>
