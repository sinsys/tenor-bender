<!-- SCRIPTS -->
<script lang="ts">
  import { onMount } from 'svelte';
  import { fly } from 'svelte/transition';
  import { elasticOut } from 'svelte/easing';

  export let visible = true;
  let init = false;
	let game = false;
  onMount(() => {
    init = true;
  });
</script>

<!-- DOM -->
<div class="app_wrapper">
  {#if init && visible}
    <div class="viewport_wrapper" transition:fly={{
      y: -256,
      duration: 2500,
      easing: elasticOut
    }}>
      <div class="title_wrapper">
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
            start
          </div>
          <div class="btn login-btn">
            login
          </div>
        </div>
      </div>
      {#if game}
        <canvas id="viewport"></canvas>
      {/if}
    </div>
  {/if}
  <div class="ocean">
    <div class="wave"></div>
    <div class="wave"></div>
  </div>
  <div class="footer">
    &copy;&nbsp;2021 - Nicholas Hazel - All rights reserved.
  </div>
</div>

<!-- STYLES -->
<style lang="scss">
  // FONTS
  $abel: 'Abel', sans-serif;
  $mulish: 'Mulish', sans-serif;
  $color-dark: #000;
  $color-blue: rgb(100,255,255);
  $color-glow: rgba(100,200,255,0.75);
  $size: 50px;

  :global(html, body) {
    margin: 0;
    padding: 0;
    font-size: 16px;
    color: #FEFEFE;
    text-shadow: 2px 2px 3px #131313;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background: #2980b9;  /* fallback for old browsers */
    background: -webkit-linear-gradient(to top, #2c3e50, #2980b9);  /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(to top, #2c3e50, #2980b9); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
    font-family: $abel;
    overflow: hidden;
  }

  .app_wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100vw;
    height: 100vh;
    max-width: 1280px;
    max-height: 768px;

    .ocean { 
      height: 5%;
      width:100%;
      position:absolute;
      bottom:0;
      left:0;
      background: #3498db;
      z-index: 999;

      .wave {
        background: url('/assets/images/wave.svg') repeat-x; 
        position: absolute;
        top: -198px;
        width: 6400px;
        height: 198px;
        animation: wave 3s cubic-bezier( 0.36, 0.45, 0.63, 0.53) infinite;
        transform: translate3d(0, 0, 0);
      }

      .wave:nth-of-type(2) {
        top: -175px;
        animation: wave 7s cubic-bezier( 0.36, 0.45, 0.63, 0.53) -.125s infinite, swell 7s ease -1.25s infinite;
        opacity: 1;
      }

      @keyframes wave {
        0% {
          margin-left: 0;
        }
        100% {
          margin-left: -1600px;
        }
      }

      @keyframes swell {
        0%, 100% {
          transform: translate3d(0,-25px,0);
        }
        50% {
          transform: translate3d(0,5px,0);
        }
      }
    }

    .footer {
      display: flex;
      position: absolute;
      justify-content: center;
      align-items: center;
      bottom: 0.5rem;
      z-index: 9999;
      text-align: center;
    }

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
          transition: all 0.3s ease-in-out;

          &:hover {
            background: rgba(255,255,255,0.3);
            border: 0.5rem solid rgba(255,255,255,0.5)
          }
        }
      }

      .title_wrapper {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        height: 100%;

        .title {
          position: relative;
          display: flex;
          text-align: center;
          font-size: 4rem;
          animation: fade-in 2s 1, title-swell 3s ease-in-out infinite;
          animation-delay: 0ms, 2000ms;

          span:nth-child(odd) {
            color: #9ebdd4;
          }

          span:nth-child(even) {
            color: #DDF;
          }

          @keyframes fade-in {
            0% {
              opacity: 0
            }
            100% {
              opacity: 1
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
          25% {height: 8.0rem; transform: translateY(2rem); background:#3498db;}
          50% {height: 0.5rem; transform: translateY(0); background:#9b59b6;}
          100% {height: 0.5rem; transform: translateY(0); background:#9b59b6;}
        }
      }

      canvas {
        width: 100%;
        height: 100%;
      }
    }
  }
</style>