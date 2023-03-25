<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import { Howl } from "howler";
  import Header from "./Header.svelte";
  import VolumeSlider from "./VolumeSlider.svelte";
	import PlayButton from "./PlayButton.svelte";
	import SkipForward from "./SkipForward.svelte";
	import SkipBackward from "./SkipBackward.svelte";
	import TimeDisplay from "./TimeDisplay.svelte";
	import type { PlaylistItem } from "$lib/types";
	import SpeedSetter from "./SpeedSetter.svelte";
	import ProgressBar from "./ProgressBar.svelte";

  export let audioUrl: string;
  export let trackTitle: string;
  export let skipForwardTimeInSeconds: number = 30;
  export let skipBackwardTimeInSeconds: number = 15;

  let audioFile: Howl;
  let currentTime: number;
  let duration: number;
  let playbackSpeed: number = 1.0;
  let isPlaying: boolean = false;
  let seekValue: number;
  let currentVolume: number = 100;
  let animationId: number;

  const soundStep = () => {
    currentTime = audioFile.seek() || 0;
    duration = audioFile.duration()

    if (audioFile.playing()) {
      animationId = requestAnimationFrame(soundStep.bind(audioFile))
    }
  }

  onMount(() => {
    audioFile = new Howl({
      src: [audioUrl],
      html5: true,
      onplay: () => {
        duration = audioFile.duration()
        isPlaying = true;
        animationId = requestAnimationFrame(soundStep.bind(audioFile))
      }
    });

    playbackSpeed = audioFile.rate()
  })

  onDestroy(() => {
    if (animationId) {
      cancelAnimationFrame(animationId)
    }
  })

  const play = (event: Event) => {
    audioFile.play()
  }

  const pause = (event: Event) => {
    audioFile.pause()
  }
  
  const seek = (event: Event) => {
    if (seekValue) {
      audioFile.seek(seekValue);
    }
  }

  const skipForward = (event: Event) => {
    audioFile.seek((currentTime || 0) + 30)
  }

  const skipBackward = (event: Event) => {
    const targetTime = (currentTime || 0) - 15;
    if (targetTime >= 0) {
      audioFile.seek(targetTime);
    } else {
      audioFile.seek(0);
    }
  }

  const onDragDone = (newValue: number) => {
    seekValue = newValue
    audioFile.seek(seekValue);
  }

  const setVolume = (value: number) => {
    audioFile.volume(value);
    currentVolume = (value * 100)
  }

  $: if (audioFile) {
    audioFile.rate(playbackSpeed);
  }
</script>

<div class="AudioPlayer flex flex-col w-full bg-slate-100">
  <ProgressBar 
    currentTimeInSeconds={currentTime}
    totalTimeInSeconds={duration}
    {onDragDone}
    />
  <div>
    <div class="mb-3 py-2">
      <Header {trackTitle} />
    </div>
    <div class="">
      <label for="volume" class="block w-32">Volume: ({currentVolume.toFixed(0)}%)</label>
      <VolumeSlider onChange={setVolume}/>
    </div>
  </div>

  <div class="AudioPlayer--controls">
    <SkipBackward onSkipBackward={skipBackward} {skipBackwardTimeInSeconds} />
    <PlayButton onPlay={play} onPause={pause} />
    <SkipForward onSkipForward={skipForward} {skipForwardTimeInSeconds} />

    <input type="number" class="border p-1 hover:border-slate-800"
      bind:value={seekValue} />
  </div>
  
  <div class="AudioPlayer--timeDisplay">
    <TimeDisplay bind:timeInSeconds={currentTime} />
    <TimeDisplay bind:timeInSeconds={duration} />
    {#if currentTime && duration}
      <span class="">{(currentTime).toFixed(3)} / {(duration).toFixed(3)}</span>
    {:else}
      <span class="">0 / 0</span>
    {/if}
  </div>
  <div class="AudioPlayer--playbackSpeed flex">
    <SpeedSetter bind:speed={playbackSpeed} />
  </div>
</div>