<script>
  import { onDestroy, onMount } from 'svelte';
  import { fly, fade } from 'svelte/transition';
	import { Link } from "svelte-navigator";
  import PageTransition from '../transitions/PageTransitions.svelte'

  export let visible = true;
  let init = false;

  onMount(() => {
    init = true;
    document.activeElement.blur();
  });

  onDestroy(() => {
    init = false;
    visible = false;
  });
</script>

<!-- DOM -->
<PageTransition>
  {#if init && visible}
    <div class="viewport_wrapper">
      <div class="title_wrapper" in:fly={{
        delay: 400,
        duration: 1500,
        y: -128
      }} out:fade>
        <div class="waveform">
          <span></span>
          <span></span>
          <span></span>
          <span></span>
          <span></span>
        </div>
        <h1 class="title">
          <span>Tenor</span>
          <span>Bender</span>
        </h1>
        <div class="button_wrapper">
          <div class="btn start-btn">
            <Link to="game">start</Link>
          </div>
          <div class="btn login-btn">
            login
          </div>
        </div>
      </div>
    </div>
  {/if}
</PageTransition>

<!-- STYLES -->
<style lang="scss">
  .viewport_wrapper {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;

    .button_wrapper {
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 9999;

      .btn {
        border: 0.5rem solid rgba(255,255,255,.1);
        background: transparent;
        padding: 0.75rem 2rem;
        margin: 0.5rem;
        border-radius: 1rem;
        font-size: 2rem;
        cursor: pointer;

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

    .title_wrapper {
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;

      .title {
        position: relative;
        display: flex;
        text-align: center;
        font-size: 4rem;
        margin-top: 1rem;
        animation: title-swell 2s ease-in-out 1s infinite;
        align-items: flex-end;
        justify-content: flex-end;

        span:nth-child(odd) {
          color: #9ebdd4;
        }

        span:nth-child(even) {
          color: #DDF;
        }

        @keyframes fade-in {
          0% {
            opacity: 0;
          }
          100% {
            opacity: 1;
          }
        }

        @keyframes title-swell {
          0%, 100% {
            transform: translate3d(0,0,0);
          }
          50% {
            transform: translate3d(0,1rem,0);
          }
        }

        @media only screen and (max-width: 600px) {
          font-size: 17.5vw;
        }
      }

      .waveform {
        position: absolute;
        margin-left: -2rem;
        
        span {
          display: block;
          bottom: -1rem;
          width: 2rem;
          height: 1rem;
          background: #9b59b6;
          position: absolute;
          animation: fade-in-wave, audio-wave;
          animation-iteration-count: 1, infinite;
          animation-duration: 2s, 1.5s;
          animation-delay: 0, 2s;
          animation-timing-function: ease-in-out;
          opacity: 0.5;

          @keyframes fade-in-wave {
            0% {
              opacity: 0
            }
            50% {
              opacity: 1
            }
            100% {
              opacity: 0.5
            }
          }
        }

        span:nth-child(1) {
          left: -6rem;
          animation-delay: 0.0s;
        }

        span:nth-child(2) {
          left: -3rem;
          animation-delay: 0.2s;
        }

        span:nth-child(3) {
          left: 0;
          animation-delay: 0.4s;
        }

        span:nth-child(4) {
          left: 3rem;
          animation-delay: 0.6s;
        }

        span:nth-child(5) {
          left: 6rem;
          animation-delay: 0.8s;
        }
      }

      @keyframes audio-wave {
        0% {height: 0.5rem; transform: translateY(0), translateX(1rem); background:#9b59b6;}
        25% {height: 8.0rem; transform: translateY(-.75rem); background:#3498db;}
        50% {height: 0.5rem; transform: translateY(0); background:#9b59b6;}
        100% {height: 0.5rem; transform: translateY(0); background:#9b59b6;}
      }
    }
  }
</style>