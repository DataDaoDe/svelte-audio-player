<script lang="ts">
  export let onPlay: (event: Event) => void;
  export let onPause: (event: Event) => void;
  export let isPlaying: boolean = false;

  const onInternalClick = (event: Event) => {
    if (isPlaying) {
      onPause(event);
      isPlaying = false;
    } else {
      onPlay(event);
      isPlaying = true;
    }
  }
</script>

<button on:click|preventDefault={onInternalClick}>
  {#if isPlaying}
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path fill="none" d="M0 0h24v24H0z"/><path d="M6 5h2v14H6V5zm10 0h2v14h-2V5z"/></svg>
  {:else}
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path fill="none" d="M0 0h24v24H0z"/><path d="M19.376 12.416L8.777 19.482A.5.5 0 0 1 8 19.066V4.934a.5.5 0 0 1 .777-.416l10.599 7.066a.5.5 0 0 1 0 .832z"/></svg>
  {/if}
</button>