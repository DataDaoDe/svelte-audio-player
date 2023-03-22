<script lang="ts">
  import { onMount } from "svelte";
  import Header from "./Header.svelte";
  import VolumeSlider from "./VolumeSlider.svelte";

  export let audioUrl: string;
  export let trackTitle: string;

  let audioFile: HTMLAudioElement;
  let currentTime: number;
  let duration: number;
  let playbackSpeed: number;
  let currentVolume: number = 100;

  onMount(() => {
    audioFile = new Audio(audioUrl);

    playbackSpeed = audioFile.playbackRate;

    audioFile.addEventListener("loadedmetadata", () => {
      duration = audioFile.duration;
    }, false);

    audioFile.addEventListener("timeupdate", () => {
      currentTime = audioFile.currentTime
    }, false);
  })

  const play = (event: Event) => {
    audioFile.play()
  }

  const pause = (event: Event) => {
    audioFile.pause()
  }

  const setVolume = (value: number) => {
    audioFile.volume = value;
    currentVolume = (value * 100)
  }

  const setSpeed = (event: Event) => {
    const elem = event.target as HTMLInputElement
    playbackSpeed = parseFloat(elem.value);
    audioFile.playbackRate = playbackSpeed;
  }
</script>

<div class="AudioPlayer flex flex-col w-full">
  <div>
    <Header {trackTitle} />
    <hr />
    <div class="">
      <label for="volume" class="block w-32">Volume: ({currentVolume.toFixed(0)}%)</label>
      <VolumeSlider onChange={setVolume}/>
    </div>
  </div>
  <div class="AudioPlayer--controls">
    <button class="border p-1 hover:border-slate-800"
      on:click|preventDefault={play}>Play</button>
    <button class="border p-1 hover:border-slate-800"
      on:click|preventDefault={pause}>Pause</button>
  </div>
  
  <div class="AudioPlayer--timeDisplay">
    {#if currentTime && duration}
      <span class="">{(currentTime).toFixed(3)} / {(duration).toFixed(3)}</span>
    {:else}
      <span class="">0 / 0</span>
    {/if}
  </div>
  <div class="AudioPlayer--playbackSpeed flex">
    <label for="set_speed" class="block w-32">Set Speed: ({playbackSpeed})</label>
    <input 
      class="w-1/2"
      type="range" min="0" max="3" step="0.25" bind:value={playbackSpeed} on:input|preventDefault={setSpeed}/>
  </div>
</div>