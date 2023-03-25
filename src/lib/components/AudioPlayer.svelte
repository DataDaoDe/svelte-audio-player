<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import { Howl } from "howler";
  import type { PlayerSettings } from "/types";
  import VolumeSlider from "./VolumeSlider.svelte";
	import PlayButton from "./PlayButton.svelte";
	import SkipForward from "./SkipForward.svelte";
	import SkipBackward from "./SkipBackward.svelte";
	import TimeDisplay from "./TimeDisplay.svelte";
	import { fade } from "svelte/transition";
	import SpeedSetter from "./SpeedSetter.svelte";
	import ProgressBar from "./ProgressBar.svelte";
	import TrackInfo from "./TrackInfo.svelte";

  export let audioUrl: string;
  export let trackTitle: string;
  export let settings: PlayerSettings = {
    skipForwardTimeInSeconds: 5,
    skipBackwardTimeInSeconds: 5
  }

  let audioFile: Howl;
  let currentTime: number;
  let duration: number;
  let playbackSpeed: number = 1.0;
  let isPlaying: boolean = false;
  let seekValue: number;
  let sliderValue: number = 0;
  let showSliderValue: boolean = false;
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

  const skipForward = (event: Event) => {
    audioFile.seek((currentTime || 0) + settings.skipForwardTimeInSeconds)
  }

  const skipBackward = (event: Event) => {
    const targetTime = (currentTime || 0) - settings.skipBackwardTimeInSeconds;
    if (targetTime >= 0) {
      audioFile.seek(targetTime);
    } else {
      audioFile.seek(0);
    }
  }

  const onDragDone = (newValue: number) => {
    seekValue = newValue
    currentTime = newValue;
    audioFile.seek(seekValue);
    showSliderValue = false;
  }

  const onMoveSlider = (newValue: number) => {
    showSliderValue = true;
    sliderValue = newValue;
  }

  const setVolume = (value: number) => {
    audioFile.volume(value);
    currentVolume = (value * 100)
  }

  $: if (audioFile) {
    audioFile.rate(playbackSpeed);
  }
</script>

<div class="AudioPlayer flex flex-row w-full bg-white">
  <div class="AudioPlayer__Navigation w-1/6 grid content-center bg-slate-100 border-b border-t border-l border-slate-400">
    <div class="mx-auto">
      <SkipBackward 
        onSkipBackward={skipBackward} 
        skipBackwardTimeInSeconds={settings.skipBackwardTimeInSeconds} />
      <PlayButton onPlay={play} onPause={pause} />
      <SkipForward onSkipForward={skipForward}
      skipForwardTimeInSeconds={settings.skipForwardTimeInSeconds} />
    </div>
  </div>
  <div class="AudioPlayer__TrackAndTimeInfo border border-slate-400 w-3/6">
    <div class="AudioPlayer__TrackAndTimeInfo_Track mt-4">
      <TrackInfo {trackTitle}/>
    </div>
    <div class="AudioPlayer__TimeProgressInfo__ProgressBar p-2">
      <div class="flex flex-row">
        <div class="text-left w-1/2">
          <TimeDisplay bind:timeInSeconds={currentTime} />
          {#if showSliderValue}
          <div in:fade out:fade class="inline text-gray-400 ml-1">
            (<TimeDisplay timeInSeconds={sliderValue}/>)
          </div>
          {/if}
        </div>
        <div class="text-right w-1/2">
          <TimeDisplay bind:timeInSeconds={duration} />
        </div>
      </div>
      <ProgressBar
        currentTimeInSeconds={currentTime}
        totalTimeInSeconds={duration}
        {onDragDone}
        {onMoveSlider}
      />
    </div>
  </div>
  <div class="AudioPlayer__Volume w-1/6">
    <VolumeSlider onChange={setVolume}/>
  </div>
  <div class="AudioPlayer__AdvancedSettings w-1/6">
    <SpeedSetter bind:speed={playbackSpeed} />
  </div>
</div>