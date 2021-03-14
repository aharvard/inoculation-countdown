<script>
  import { onMount } from "svelte";

  const countDownDate = new Date("Apr 23, 2021 9:15:00").getTime();

  $: now = new Date();
  $: distance = countDownDate - now;

  $: days = Math.floor(distance / (1000 * 60 * 60 * 24));
  $: seconds = Math.floor((distance % (1000 * 60)) / 1000);
  $: minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
  $: hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));

  onMount(() => {
    const interval = setInterval(() => {
      now = new Date();
    }, 1000);

    return () => {
      clearInterval(interval);
    };
  });
</script>

<main>
  <p id="days">
    <span class="number">
      {days}
    </span>
    days
  </p>
  <p id="hours">
    <span class="number">
      {hours}
    </span>
    hours
  </p>
  <p id="mins">
    <span class="number">
      {minutes}
    </span>
    min
  </p>
  <p id="secs">
    <span class="number">
      {seconds}
    </span>
    secs
  </p>
  <p id="remain">untill we are free!</p>
</main>

<style>
  :global(body) {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
      Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
    margin: 0;
    padding: 0;

    background: var(--background);
    color: var(--text);
  }

  main {
    font-size: clamp(1rem, 2vw, 2rem);

    height: 100vh;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: 2fr 1fr 0.75fr;
    grid-template-areas:
      "days days days"
      "hours mins secs"
      "remain remain remain";
  }

  p {
    outline: 1px solid;
    margin: 0;
    display: grid;
    place-content: center;
    text-align: center;
    text-transform: uppercase;
    letter-spacing: 0.25ch;
    font-weight: 900;
    line-height: 1;
  }

  .number {
    font-size: 300%;
    font-weight: 200;
  }

  #days {
    grid-area: days;
  }

  #days .number {
    font-size: 800%;
    font-weight: 100;
  }

  #hours {
    grid-area: hours;
  }
  #mins {
    grid-area: mins;
  }
  #secs {
    grid-area: secs;
  }
  #remain {
    grid-area: remain;
    font-weight: 700;
  }
</style>
