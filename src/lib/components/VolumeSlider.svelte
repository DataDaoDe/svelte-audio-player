<script lang="ts">
  import type { FormEventHandler } from "svelte/types"

  export let onChange: (newValue: number) => void;

  const setVolumeInternal = (event: Event) => {
    const elem: HTMLInputElement = event.target as HTMLInputElement;
    onChange(parseInt(elem.value, 10) / 100.0);
  }
</script>


<input 
  on:input|preventDefault={setVolumeInternal}
  type="range" min="0" max="100" value="100" id="AudioPlayer--volumeSlider">