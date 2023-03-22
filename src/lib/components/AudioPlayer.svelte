<script lang="ts">
  import { onMount } from "svelte";
  import Header from "./Header.svelte";
  import VolumeSlider from "./VolumeSlider.svelte";

  export let audioUrl: string;
  export let trackTitle: string;

  let audioFile: HTMLAudioElement;

  onMount(() => {
    audioFile = new Audio(audioUrl);

    audioFile.addEventListener("loadedmetadata", () => {
      console.log(audioFile.duration / (60.0 * 60));
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
  }
</script>

<div class="audio-player flex">
  <Header {trackTitle} />
  <div class="audio-player--controls">
    <button on:click|preventDefault={play}>Play</button>
    <button on:click|preventDefault={pause}>Pause</button>
  </div>
  <VolumeSlider onChange={setVolume}/>
</div>