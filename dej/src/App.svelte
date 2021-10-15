<script lang="ts">
  import Timer from "./components/Timer.svelte";
  import AlertWithSound from "./components/AlertWithSound.svelte";

  let message = "";

  function saveMessage(e) {
    message = e.detail.message;
  }
  function closeMessage(e) {
    message = "";
  }
</script>

<div class="page">
  <header>
    <div id="logo">DEJ</div>
  </header>

  <main>
    {#if message !== ""}
      <AlertWithSound {message} on:alertClose={closeMessage} />
    {/if}
    <Timer h={0} m={0} s={3} on:alertWithSound={saveMessage} />
  </main>

  <footer>
    &nbsp;
  </footer>
</div>

<style>
</style>
