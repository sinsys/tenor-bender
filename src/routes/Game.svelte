<script type="ts">
  import PageTransition from '../transitions/PageTransitions.svelte'
  import { Link } from "svelte-navigator";
</script>

<PageTransition>
  <div class="game_wrapper">
    <div class="button_wrapper">
      <div class="btn start-btn">
        <Link to="/">	
          &#10094;&nbsp;back
        </Link>
      </div>
    </div>
    <canvas id="viewport"></canvas>
  </div>
</PageTransition>

<style lang="scss">
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
      top: 0;
      left: 0;
      justify-content: center;
      align-items: center;
      z-index: 9999;

      .btn {
        border: 0.5rem solid rgba(255,255,255,.1);
        background: transparent;
        padding: 0.75rem 2rem;
        margin: 0.5rem;
        border-radius: 1rem;
        font-size: 1.5rem;
        cursor: pointer;
        transition: all 0.3s ease-in-out;
        display: flex;
        justify-content: center;
        align-items: center;

        &.start-btn {
          position: absolute;
        }
        &:hover {
          background: rgba(255,255,255,0.3);
          border: 0.5rem solid rgba(255,255,255,0.5)
        }
        > :global(a) {
            text-decoration: none;
            color: #FEFEFE;
        }
      }
    }
  }
</style>