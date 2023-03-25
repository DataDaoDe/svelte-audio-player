<script lang="ts">

  export let onChange: (newValue: number) => void;
  export let volume: number;
  let progressBarPercent = volume || 100;

  const setVolumeInternal = (event: Event) => {
    const elem: HTMLInputElement = event.target as HTMLInputElement;
    let newVolume = parseInt(elem.value, 10);
    progressBarPercent = newVolume;
    onChange(newVolume / 100.0);
  }

  $: console.log(volume)
</script>

<div class="VolumeSlider">
  <div class="flex flex-row p-2">
  <span class="VolumeSlider--volumeDownIcon mr-1">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 22" width="22" height="22">
      <path fill="none" d="M0 0h24v24H0z"/>
      <path fill="#bfbfbf" d="M8.889 16H5a1 1 0 0 1-1-1V9a1 1 0 0 1 1-1h3.889l5.294-4.332a.5.5 0 0 1 .817.387v15.89a.5.5 0 0 1-.817.387L8.89 16zm9.974.591l-1.422-1.422A3.993 3.993 0 0 0 19 12c0-1.43-.75-2.685-1.88-3.392l1.439-1.439A5.991 5.991 0 0 1 21 12c0 1.842-.83 3.49-2.137 4.591z"/></svg>
  </span>
  <input 
    on:input|preventDefault={setVolumeInternal}
    type="range"
    class="w-full rounded-lg h-[4px] mt-[9px]"
    style="background: linear-gradient(to right, #b5b5b5 0%, #b5b5b5 {progressBarPercent}%, #e5e5e5 {progressBarPercent}%, #e5e5e5 100%)"
    min="0"
    max="100"
    value="{volume}"
    id="AudioPlayer--volumeSlider"
  >
  <span class="VolumeSlider--volumeUpIcon ml-1">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 22" width="22" height="22">
      <path fill="none" d="M0 0h24v24H0z"/>
      <path fill="#bfbfbf" d="M5.889 16H2a1 1 0 0 1-1-1V9a1 1 0 0 1 1-1h3.889l5.294-4.332a.5.5 0 0 1 .817.387v15.89a.5.5 0 0 1-.817.387L5.89 16zm13.517 4.134l-1.416-1.416A8.978 8.978 0 0 0 21 12a8.982 8.982 0 0 0-3.304-6.968l1.42-1.42A10.976 10.976 0 0 1 23 12c0 3.223-1.386 6.122-3.594 8.134zm-3.543-3.543l-1.422-1.422A3.993 3.993 0 0 0 16 12c0-1.43-.75-2.685-1.88-3.392l1.439-1.439A5.991 5.991 0 0 1 18 12c0 1.842-.83 3.49-2.137 4.591z"/></svg>
  </span>
</div>
</div>

<style>
  input[type="range"] {
    -webkit-appearance: none;
    appearance: none;
    cursor: pointer;
    background: linear-gradient(to right, #cecece 0%, #cecece 0%, #e5e5e5 0%, #e5e5e5 100%);
    transition: background 450ms ease-in;
  }

  /***** Focus Styles *****/
  /* Removes default focus */
  input[type="range"]:focus {
    outline: none;
  }

  /** TRACK STYLES */
  /***** Chrome, Safari, Opera, and Edge Chromium *****/
  input[type="range"]::-webkit-slider-runnable-track {}
  /******** Firefox ********/
  input[type="range"]::-moz-range-track {}

  /** THUMB STYLES */
  /***** Chrome, Safari, Opera, and Edge Chromium *****/
  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none; /* Override default look */
    appearance: none;
    margin-top: -1px; /* Centers thumb on the track */
    background-color: #f5f5f5;
    border: 1px solid #b2b2b2;
    border-radius: 28px;
    height: 14px;
    width: 14px;    
  }
  /***** Firefox *****/
  input[type="range"]::-moz-range-thumb {
      border: none; /*Removes extra border that FF applies*/
      border-radius: 0; /*Removes default border-radius that FF applies*/
      background-color: #f5f5f5;
      border: 1px solid #b2b2b2;
      border-radius: 28px;
      height: 14px;
      width: 14px;   
  }
</style>