<script lang="ts">
  export let currentTimeInSeconds: number;
  export let totalTimeInSeconds: number;
  export let onDragDone: (newTimeValue: number) => void;
  export let onMouseEnter: (event: Event) => void = (_event) => {};
  export let onMouseLeave: (event: Event) => void = (_event) => {};
  export let onMoveSlider: (timeValue: number) => void = (_event) => {};

  let value = currentTimeInSeconds || 0;
  let progresBarPercent = 0;

  let dragging = false;

  const onInternalDrag = (event: Event) => {
    dragging = false;
    onDragDone(value);
    progresBarPercent = value / totalTimeInSeconds * 100;
  }

  const onInternalStartDrag = (event: Event) => {
    dragging = true;
  }

  const onInternalMouseMove = (event: MouseEvent) => {
    if (dragging) {
      onMoveSlider(value);
      progresBarPercent = value / totalTimeInSeconds * 100;
    }
  }

  $: progresBarPercent = (currentTimeInSeconds / totalTimeInSeconds) * 100;
</script>


{#if currentTimeInSeconds && totalTimeInSeconds}
  <div id="audio-progress" class="audio-progress">
    <input
      class="w-full bg-slate-50"
      type="range"
      style="background: linear-gradient(to right, #737373 0%, #737373 {progresBarPercent}%, #e5e5e5 {progresBarPercent}%, #e5e5e5 100%)"
      max={totalTimeInSeconds}
      bind:value={value}
      on:mouseenter={onMouseEnter}
      on:mousemove={onInternalMouseMove}
      on:mouseleave={onMouseLeave}
      min={0} 
      on:mousedown={onInternalStartDrag}
      on:mouseup={onInternalDrag} 
      />
  </div>
{:else}
  <div id="audio-progress--noTimeValue" class="audio-progress--noTimeValues">
    <input
      class="w-full bg-slate-50"
      type="range"
      style="background: linear-gradient(to right, #737373 0%, #737373 0%, #e5e5e5 0%, #e5e5e5 100%)"
      max={100}
      disabled
      value={0}
      min={0} on:mouseup={onInternalDrag} />
  </div>
{/if}

<style>
  input[type="range"] {
    -webkit-appearance: none;
    appearance: none;
    cursor: pointer;
    background: linear-gradient(to right, #737373 0%, #737373 0%, #e5e5e5 0%, #e5e5e5 100%);
    transition: background 450ms ease-in;
  }

  /***** Focus Styles *****/
  /* Removes default focus */
  input[type="range"]:focus {
    outline: none;
  }

  /** TRACK STYLES */
  /***** Chrome, Safari, Opera, and Edge Chromium *****/
  input[type="range"]::-webkit-slider-runnable-track {
    height: 0.3rem;
  }
  /******** Firefox ********/
  input[type="range"]::-moz-range-track {
    background: #e5e5e5;
    height: 0.3rem;
  }

  /** THUMB STYLES */
  /***** Chrome, Safari, Opera, and Edge Chromium *****/
  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none; /* Override default look */
    appearance: none;
    margin-top: -6px; /* Centers thumb on the track */
    background-color: #707070;
    border-left: 1px solid white;
    border-right: 2px solid #202020;
    height: 0.7rem;
    width: 0.4rem;    
  }
  /***** Firefox *****/
  input[type="range"]::-moz-range-thumb {
      border: none; /*Removes extra border that FF applies*/
      border-radius: 0; /*Removes default border-radius that FF applies*/
      background-color: #707070;
      border-left: 1px solid white;
      border-right: 1px solid #343434;
      height: 0.7rem;
      width: 0.3rem;
  }
</style>