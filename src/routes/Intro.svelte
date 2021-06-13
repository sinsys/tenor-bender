<script>
  import { onDestroy, onMount } from 'svelte';
  import { fly, fade } from 'svelte/transition';
	import { Link } from "svelte-navigator";
  import PageTransition from '../transitions/PageTransitions.svelte'
  import Footer from '../components/Footer.svelte'

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
          <Link to="game">
            <div class="btn start-btn">
              start
            </div>
          </Link>
          <div class="btn login-btn">
            login
          </div>
        </div>
      </div>
    </div>
    <Footer />
  {/if}
  
</PageTransition>

<!-- STYLES -->
<style lang="scss">
  @import '../styles/global.scss';
  
  .viewport_wrapper {
    width: 100%;
    height: 100%;
    @include flexColumnCenter;

    .button_wrapper {
      @include flexRowCenter;
      z-index: 9999;

      > :global(a) {
        text-decoration: none;
        color: #FEFEFE;
      }

      .btn {
        @include btnBase;
        @include btnBorderTransparent;
        &:hover {
          @include btnBorderActive;
        }
      }
    }

    .title_wrapper {
      @include flexColumnCenter;

      .title {

        display: flex;
        font-size: 4rem;
        animation: title-swell 2s ease-in-out 1s infinite;
        border: transparent;
        outline: none;
        text-shadow: 2px 2px 1rem rgba(0,0,0,0.5);
        
        span:nth-child(odd) {
          color: #9ebdd4;
        }

        span:nth-child(even) {
          color: #DDF;
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
          background: $magenta;
          position: absolute;
          animation: fade-in-wave, audio-wave;
          animation-iteration-count: 1, infinite;
          animation-duration: 2s, 1.5s;
          animation-delay: 0, 2s;
          animation-timing-function: ease-in-out;
          opacity: 0.5;
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
    }
  }
</style>