<script lang="ts">
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let message = "";

  function close() {
    dispatch("alertClose");
  }
</script>

<div class="alert">
  <audio src="/audio/alarm.wav" id="alarm" autoplay={message !== ""} />
  <button on:click={() => close()}>&times;</button>
  {message}
</div>

<!-- alarm sound file from https://freesound.org/s/198841/ -->
