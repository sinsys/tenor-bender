<script>
	import { cubicInOut } from 'svelte/easing'
	let duration = 1000
	let delay = duration

	const transitionIn = () => ({
		duration,
		delay,
		easing: cubicInOut,
		css: (t) => `opacity: ${t}`,
	})

	const transitionOut = () => ({
		duration,
		delay: 0,
		easing: cubicInOut,
		css: (t) => `opacity: ${t}`,
	})
</script>

<div in:transitionIn out:transitionOut class="page">
	<slot />
</div>

<style type="scss">
	.page {
		width: 100vw;
		height: 100vh;
		position: absolute;
		display: flex;
		align-items: center;
		justify-content: center;
		overflow: hidden;
	}
</style>