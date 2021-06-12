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
      y: -128,
      duration: 2500,
      easing: elasticOut
    }}>
      <h1 class="title">
        <span>Tenor</span>
        <span>Bender</span>
      </h1>
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
    background: #015871;
  }

  .wave {
    background: url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/85486/wave.svg) repeat-x; 
    position: absolute;
    top: -198px;
    width: 6400px;
    height: 198px;
    animation: wave 7s cubic-bezier( 0.36, 0.45, 0.63, 0.53) infinite;
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

      .title {
        text-align: center;
        margin: 0 1rem;
        font-size: 3.5rem;
        span:nth-child(odd) {
          color: #C8B6F5;
        }
        span:nth-child(even) {
          color: #DDF;
        }
      }
      canvas {
        width: 100%;
        height: 100%;
      }
    }
  }
</style>