<script>
  import AudioPlayer from "../lib/components/AudioPlayer.svelte";

  import { audioData } from "../data/audioData";
</script>

<AudioPlayer 
  audioUrl={audioData[0].url}
  trackTitle={audioData[0].name}
/>