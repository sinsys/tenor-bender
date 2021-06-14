<script type="ts">
  import { onMount } from 'svelte'
  let audio: HTMLAudioElement
  let canvas: HTMLCanvasElement

  // Audio
  let context: AudioContext
  let analyser: AnalyserNode
  let source: MediaElementAudioSourceNode

  // Canvas
  let ctx: CanvasRenderingContext2D;
	let fbc_array: Uint8Array
	let bar_count: number
  let bar_pos: number
  let bar_width: number
  let bar_height: number
  let invited: boolean = false
  let wH:number = window.innerHeight
  let wW:number = window.innerWidth
  const start = (): void => {
    invited = true;
    context.resume();
    audio.play();
  }
  const FrameLooper = (): void => {
    fbc_array = new Uint8Array(analyser.frequencyBinCount)
    bar_count = 16
    analyser.getByteFrequencyData(fbc_array)
    ctx.clearRect(0, 0, wW, wH || 0)

    // Calculate bar heights
    let bars = []
    for (let i = 0; i < bar_count; i++) {
      let chunkSize = Math.floor(fbc_array.length / bar_count)
      let chunk = fbc_array.slice(chunkSize * i, chunkSize * i + chunkSize)
      bars.push(chunk.reduce((acc, i) => acc += i, 0) / chunk.length)
    }
    for (var bar = 1; bar < bars.length + 1; bar++) {
      const barFreq = bars[bar]
      if (barFreq < 30) {
        continue;
      }
      if (barFreq < 75) {
        ctx.globalAlpha = 0.3
        ctx.fillStyle = "9999FF"
      } else if (barFreq < 150) {
        ctx.globalAlpha = 1
        ctx.fillStyle = "#3498db"
      } else {
        ctx.globalAlpha = 0.3
        ctx.fillStyle = "CCCCFF"
      }
      bar_height = (-(wH / 256) * barFreq) * 2

      // Offsetting 1 pixel each side
      bar_pos = bar * wW / bar_count + 1
      bar_width = (wH / bar_count) + 2
      
      ctx.fillRect(bar_pos, canvas?.height || 0, bar_width, bar_height)
    }
    setTimeout(() => {
      FrameLooper();
    }, 10);
  }
  onMount(() => {
    /* SETUP */
    // Audio config
    audio.src = '/assets/sample.mp3'
    audio.controls = true
    audio.loop = false
    audio.autoplay = false
    context = new AudioContext()
    analyser = context.createAnalyser()
    source = context.createMediaElementSource(audio)
    // Canvas
    ctx = canvas.getContext('2d');
    
    // Connections
    analyser.connect(context.destination)
    source.connect(analyser)
		canvas.width = window.innerWidth
		canvas.height = window.innerHeight
    let frame = requestAnimationFrame(FrameLooper);
		return () => {
			cancelAnimationFrame(frame)
		}
	})
</script>

<audio bind:this={audio}>
  <track kind="captions">
</audio>
<canvas bind:this={canvas} on:click={start} id="viewport"></canvas>

<style lang="scss">
  audio {
    position: absolute;
    z-index: 9999;
    bottom: 0.5rem;
    left: 0.5rem;
    right: 0.5rem;
    width: calc(100vw - 1rem);
  }
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
    z-index: 999;
  }
</style>