<script lang="ts">
  import Modal from "./Modal.svelte";
  export let trackTitle: string;

  let showModal = false;

  const showTrackInfo = (event: Event) => {
    showModal = true;
  }
</script>

<div class="mx-auto text-center">
  <div class="space-x-1">
    <h3 class="truncate text-slate-600 inline-block">{trackTitle}</h3>
    <button on:click={showTrackInfo}>
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path fill="none" d="M0 0h24v24H0z"/><path d="M4.5 10.5c-.825 0-1.5.675-1.5 1.5s.675 1.5 1.5 1.5S6 12.825 6 12s-.675-1.5-1.5-1.5zm15 0c-.825 0-1.5.675-1.5 1.5s.675 1.5 1.5 1.5S21 12.825 21 12s-.675-1.5-1.5-1.5zm-7.5 0c-.825 0-1.5.675-1.5 1.5s.675 1.5 1.5 1.5 1.5-.675 1.5-1.5-.675-1.5-1.5-1.5z"/></svg>
    </button>
  </div>
</div>

<Modal bind:showModal>
  <h2 slot="header" class="text-2xl">Track Info</h2>
  <p>Regular Content</p>
</Modal>