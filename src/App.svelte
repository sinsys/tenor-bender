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
    <div class="ocean">
      <div class="wave"></div>
      <div class="wave"></div>
    </div>
    <div class="viewport_wrapper" transition:fly={{
      y: -256,
      duration: 2500,
      easing: elasticOut
    }}>
      <div class="title-wrapper">
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
      </div>

      {#if game}
        <canvas id="viewport"></canvas>
      {/if}
    </div>
  {/if}

</div>

<!-- STYLES -->
<style lang="scss">
  // FONTS
  $abel: 'Abel', sans-serif;
  $mulish: 'Mulish', sans-serif;

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

  .ocean { 
    height: 5%;
    width:100%;
    position:absolute;
    bottom:0;
    left:0;
    background: #3498db;
  }

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

  .app_wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100vw;
    height: 100vh;
    max-width: 1280px;
    max-height: 768px;

    .viewport_wrapper {
      width: 100%;
      height: 100%;

      .title-wrapper {
        display: flex;
        align-items: center;
        justify-content: center;
        height: auto;

        .title {
          position: relative;
          display: flex;
          text-align: center;
          margin: 0 1rem;
          font-size: 17.5vw;
          @media only screen and (min-width: 600px) {
            font-size: 4rem;
          }
          span:nth-child(odd) {
            color: #9ebdd4;
          }

          span:nth-child(even) {
            color: #DDF;
          }
        }

        .waveform {
          position: absolute;
          margin-left: -36px;

          span {
            display: block;
            bottom: -36px;
            width: 36px;
            height: 16px;
            background: #9b59b6;
            position: absolute;
            animation: audio-wave 1.5s infinite ease-in-out;
          }

          span:nth-child(1) {
            left: -96px;
            animation-delay: 0.0s;
          }
          span:nth-child(2) {
            left: -48px;
            animation-delay: 0.2s;
          }

          span:nth-child(3) {
            left: 0;
            animation-delay: 0.4s;
          }

          span:nth-child(4) {
            left: 48px;
            animation-delay: 0.6s;
          }

          span:nth-child(5) {
            left: 96px;
            animation-delay: 0.8s;
          }
        }

        @keyframes audio-wave {
          0% {height:5px;transform:translateY(0px);background:#9b59b6;}
          25% {height:128px;transform:translateY(20px);background:#3498db;}
          50% {height:5px;transform:translateY(0px);background:#9b59b6;}
          100% {height:5px;transform:translateY(0px);background:#9b59b6;}
        }
      }

      canvas {
        width: 100%;
        height: 100%;
      }
    }
  }
</style>