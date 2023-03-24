<script lang="ts">
  export let speed: number;
  export let stepSize: number = 0.05;
  export let speedMax: number = 4;
  export let speedMin: number = 0.5;

  let showMenu = false;

  const toggleSpeedMenu = (event: Event) => {
    showMenu = !showMenu;
  }

  const setSpeed = (increase: boolean) => {
    if (increase) {
      speed = Math.min(speedMax, speed + stepSize);
    } else {
      speed = Math.max(speedMin, speed - stepSize);
    }
  } 
</script>

<div>
<div class="">
  <button
    title="Set playback speed"
    aria-label="Set playback speed"
    on:click|preventDefault={toggleSpeedMenu}>
    {#if showMenu}
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path fill="none" d="M0 0h24v24H0z"/><path d="M6.382 5.968A8.962 8.962 0 0 1 12 4c2.125 0 4.078.736 5.618 1.968l1.453-1.453 1.414 1.414-1.453 1.453a9 9 0 1 1-14.064 0L3.515 5.93l1.414-1.414 1.453 1.453zM12 20a7 7 0 1 0 0-14 7 7 0 0 0 0 14zm1-8h3l-5 6.5V14H8l5-6.505V12zM8 1h8v2H8V1z"/></svg>
    {:else}
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path fill="none" d="M0 0h24v24H0z"/><path d="M6.382 5.968A8.962 8.962 0 0 1 12 4c2.125 0 4.078.736 5.618 1.968l1.453-1.453 1.414 1.414-1.453 1.453a9 9 0 1 1-14.064 0L3.515 5.93l1.414-1.414 1.453 1.453zM13 12V7.495L8 14h3v4.5l5-6.5h-3zM8 1h8v2H8V1z"/></svg>
    {/if}
  </button>
</div>
{#if showMenu}
  <div class="speedMenu flex flex-col border p-4 rounded border-slate-500">
    <div class="grid grid-cols-2 mb-2">
      <div>Speed:</div>
      <div class="text-right">{speed.toFixed(2)}</div>
    </div>
    <div class="flex">
      <div class="slower rounded-full border border-slate-500">
        <button
            class="align-middle"
            title="Decrease playback speed"
            aria-label="Decrease playback speed"
            on:click|preventDefault={() => setSpeed(false)}>
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path fill="none" d="M0 0h24v24H0z"/><path d="M5 11h14v2H5z"/></svg>
        </button>
      </div>
      <div class="speedBar">
        <input
        class="w-full mt-2"
        type="range" min="0" max="4" step="0.05" bind:value={speed} />
      </div>
      <div class="faster rounded-full border border-slate-500">
        <button
            class="align-middle"
            title="Increase playback speed"
            aria-label="Increase playback speed"
            on:click|preventDefault={() => setSpeed(true)}>
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path fill="none" d="M0 0h24v24H0z"/><path d="M11 11V5h2v6h6v2h-6v6h-2v-6H5v-2z"/></svg>
        </button>
      </div>
    </div>
  </div>
{/if}
</div>