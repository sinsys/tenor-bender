<script type="ts">
  import { fly } from 'svelte/transition'
  import { cubicInOut } from 'svelte/easing'
</script>

<div class="ocean" out:fly="{{ y: 200, duration: 1000 }}">
  <div class="wave"></div>
  <div class="wave"></div>
</div>
<div class="footer" out:fly="{{ y: 200, duration: 1000, easing: cubicInOut }}">
  &copy;&nbsp;2021 - Nicholas Hazel - All rights reserved.
</div>

<style type="scss">
  @import '../styles/global.scss';

  .ocean { 
    height: 10vh;
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
</style>