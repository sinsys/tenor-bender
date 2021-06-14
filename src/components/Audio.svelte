<script lang="typescript">
  import { onMount } from 'svelte'
  
  let audio: HTMLAudioElement
  let canvas: HTMLCanvasElement

  let source: MediaElementAudioSourceNode
  // Audio
  let context: AudioContext
  let analyser: AnalyserNode

  // Canvas
  let ctx: CanvasRenderingContext2D;
	let fbc_array: Uint8Array
	let bar_count: number
  let invited: boolean = false
  let wH:number = window.innerHeight
  let wW:number = window.innerWidth

  // const drawBuffer = (width: number, height: number, context: CanvasRenderingContext2D, buffer): void => {
  //   var data = buffer.getChannelData(0);
  //   var step = Math.ceil(data.length / width);
  //   var amp = height / 2;
  //   context.fillStyle = "#FFFFFF"
  //   for (var i = 0; i < width; i++) {
  //     var min = 1.0;
  //     var max = -1.0;
  //     for (var j = 0; j < step; j++) {
  //       var datum = data[(i * step) + j]; 
  //       if (datum < min) {
  //         min = datum;
  //       }
  //       if (datum > max) {
  //         max = datum;
  //       }
  //       context.fillRect(
  //         i, (1 + min) * amp,
  //         1, Math.max(1, (max-min) * amp)
  //       );
  //     }
  //   }
  // }

  // const initAudio = async (): Promise<void> => {
  //   var audioRequest = new XMLHttpRequest();
  //   audioRequest.open("GET", "/assets/sample2.mp3", true);
  //   audioRequest.responseType = "arraybuffer";
  //   audioRequest.onload = function() {
  //     context.decodeAudioData(
  //       audioRequest.response, 
  //       function(buffer) { 
  //         drawBuffer(wW * 100, canvas.height, ctx, buffer);
  //       }
  //     );
  //   }
  //   audioRequest.send();
  // }

  const start = async (): Promise<void> => {
    invited = true;
    context.resume();
    audio.play()
    // await initAudio();
  }

  const FrameLooper = (): void => {
    fbc_array = new Uint8Array(analyser.frequencyBinCount)
    bar_count = 5
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
      const barFreq = bars[bar - 1]
      let bar_height = (-(wH / 256) * barFreq) * 1.5,
        bar_width = (wW / bar_count) - 4,
        bar_pos = (bar * wW / bar_count) - (bar_width / 2)
      if (barFreq < 40) {
        ctx.globalAlpha = 0.3
        ctx.fillStyle = "#9999FF"
      } else {
        ctx.globalAlpha = 1
        ctx.fillStyle = "#3498db"
      }
      switch (bar) {
        case 1: {
          ctx.fillRect(bar_pos, canvas?.height || 0, bar_width, bar_height * 0.66)
          break;
        }
        case 2: {
          ctx.fillRect(bar_pos, canvas?.height || 0, bar_width, bar_height * 1)
          break;
        }
        case 3: {
          ctx.fillRect(bar_pos, canvas?.height || 0, bar_width, bar_height * 1.33)
          break;
        }
        case 4: {
          ctx.fillRect(bar_pos, canvas?.height || 0, bar_width, bar_height * 1.66)
          break;
        }
      }
    }
    setTimeout(() => {
      FrameLooper();
    }, 10);
  }

  onMount(() => {
    /* SETUP */
    // Audio config
    audio.src = '/assets/sample2.mp3'
    audio.controls = false
    audio.loop = false
    audio.autoplay = false
    context = new (window.AudioContext || (window as any).webkitAudioContext);
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
  @import '../styles/global';
  #viewport {
    position: absolute;
    height: 100vh;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: #131313;
    z-index: 999;
  }
</style>