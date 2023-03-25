<script>
  import AudioPlayer from "../lib/components/AudioPlayer.svelte";

  import { audioData } from "../data/audioData";
</script>

<div class="w-3/4 mx-auto border p-4 mt-5">
<AudioPlayer 
  audioUrl={audioData[0].url}
  trackTitle={audioData[0].name}
/>
</div>