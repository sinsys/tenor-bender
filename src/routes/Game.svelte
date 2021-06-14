<script type="ts">
  import PageTransition from '../transitions/PageTransitions.svelte'
  import Audio from '../components/Audio.svelte'
  import { Link } from "svelte-navigator";
</script>

<PageTransition>
  <div class="game_wrapper">
    <div class="button_wrapper">
      <Link to="/">
        <div class="btn back-btn">
          &#10094;&nbsp;back
        </div>
      </Link>
    </div>
    <Audio></Audio>
  </div>
</PageTransition>

<style lang="scss">
  @import '../styles/global';
  #viewport {
    position: absolute;
    width: 100vw;
    height: 100vh;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: #131313;
  }
  .game_wrapper {
    width: 100vw;
    height: 100vh;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: flex-start;
    align-items: flex-start;
    flex-direction: column;

    .button_wrapper {
      position: absolute;
      top: 2rem;
      left: 1rem;
      justify-content: center;
      align-items: center;
      z-index: 9999;

      > :global(a) {
        text-decoration: none;
        color: #FEFEFE;
      }

      .btn {
        @include btnBase;
        @include btnBorderTransparent;
        &.back-btn {
          position: absolute;
        }

        &:hover {
          background: rgba(255,255,255,0.3);
          @include btnBorderActive;
        }
      }
    }
  }
</style>