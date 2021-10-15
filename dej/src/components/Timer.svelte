<script lang="ts">
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let h: number;
  export let m: number;
  export let s: number;

  let totalSeconds = getTotalSeconds(h, m, s);
  let current = getTotalSeconds(h, m, s);
  let timer = null;
  $: countDown = display(current);
  let started = false;

  function getTotalSeconds(h: number, m: number, s: number): number {
    return h * 60 * 60 + m * 60 + s;
  }

  function getHumanTime(seconds: number): { h: number; m: number; s: number } {
    let s = seconds % 60;
    let h = Math.floor(seconds / 60 / 60);
    let m = (seconds - s - h * 60 * 60) / 60;

    return { h, m, s };
  }

  function start() {
    timer = setInterval(() => {
      started = true;
      current--;
      //countDown = display();

      if (current === 0) {
        started = false;
        current = totalSeconds;
        //countDown = display();

        dispatch("alertWithSound", { message: "Done!" });
        clearInterval(timer);
        timer = null;
      }
    }, 1000);
  }

  function pause() {
    clearInterval(timer);
    started = false;
  }

  function stop() {
    clearInterval(timer);
    timer = null;
    current = totalSeconds;
    started = false;
    //countDown = display();
  }

  function display(current: number) {
    let time = getHumanTime(current);
    let hour = "";
    if (time.h > 0) {
      hour = `${time.h}:`;
    }
    let min = `${time.m}`.padStart(2, "0");
    let sec = `${time.s}`.padStart(2, "0");

    return `${hour}${min}:${sec}`;
  }
</script>

<div id="timer">
  {countDown}
  <button on:click={() => start()} disabled={started}>Start</button>
  <button on:click={() => pause()} disabled={!started}>Pause</button>
  <button on:click={() => stop()} disabled={!started}>Stop</button>
</div>

<style>
</style>
