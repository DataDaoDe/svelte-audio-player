<script lang="ts">

  export let onChange: (newValue: number) => void;

  const setVolumeInternal = (event: Event) => {
    const elem: HTMLInputElement = event.target as HTMLInputElement;
    onChange(parseInt(elem.value, 10) / 100.0);
  }
</script>


<input 
  on:input|preventDefault={setVolumeInternal}
  type="range"
  class="w-full"
  min="0"
  max="100"
  value="100"
  id="AudioPlayer--volumeSlider"
>