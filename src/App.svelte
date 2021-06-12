<!-- SCRIPTS -->
<script lang="ts">
	import { Router, Route } from "svelte-navigator";
  import Intro from "./routes/Intro.svelte";
	import Game from "./routes/Game.svelte";

  import Footer from './components/Footer.svelte'
  import Sandbox from "./Sandbox.svelte";
</script>

<Router>
	<div class="app_wrapper">
		<Route path="/">
      <Intro />
    </Route>
		<Route path="game">
      <Game />
    </Route>
    <Route path="sandbox" component={Sandbox} />
	</div>
  <Footer />
</Router>

<style lang="scss">
  // FONTS
  $abel: 'Abel', sans-serif;
  $mulish: 'Mulish', sans-serif;
  :global(html, body) {
    margin: 0;
    padding: 0;
    font-size: 16px;
    color: #FEFEFE;
    text-shadow: 2px 2px 8px #131313;
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

  :global(div) {
    outline-style:none;
  }
  
  :global(*,*:focus,*:hover) {
    outline:none;
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
  }
</style>