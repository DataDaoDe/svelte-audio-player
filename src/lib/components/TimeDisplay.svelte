<script lang="ts">
  export let timeInSeconds: number;

  let hours: number = 0;
  let minutes: number = 0;
  let seconds: number = 0;

  const formatTime = (time: number) => {
    if (time) {
      hours = Math.floor((time / 60) / 60);
      minutes = Math.floor((time/60) % 60);
      seconds = Math.floor(time % 60);
    }
  }

  $: formatTime(timeInSeconds);
  $: hoursDisplay = String(hours).padStart(2, '0')
  $: minutesDisplay = String(minutes).padStart(2, '0')
  $: secondsDisplay = String(seconds).padStart(2, '0')
</script>

<div class="text-sm text-gray-400 inline">
  <span class="hours">{hoursDisplay}</span>
  <span class="sep">:</span>
  <span class="minutes">{minutesDisplay}</span>
  <span class="sep">:</span>
  <span class="seconds">{secondsDisplay}</span>
</div>