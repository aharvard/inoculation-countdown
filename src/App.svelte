<script>
  import { onMount } from "svelte";

  const firstShotDate = new Date("Mar 10, 2021 11:15:00").getTime();
  const fullInocDate = new Date("Apr 23, 2021 9:15:00").getTime();
  const inocPeriod = fullInocDate - firstShotDate;

  $: now = new Date();
  $: timeLeft = fullInocDate - now;

  $: days = Math.floor(timeLeft / (1000 * 60 * 60 * 24));
  $: seconds = Math.floor((timeLeft % (1000 * 60)) / 1000);
  $: minutes = Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60));
  $: hours = Math.floor((timeLeft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));

  const plural = (num, str) => (num === 1 ? str : str + "s");

  $: daysLabel = plural(days, "day");
  $: secondsLabel = plural(seconds, "second");
  $: minutesLabel = plural(minutes, "minute");
  $: hoursLabel = plural(hours, "hour");

  $: inocWaitingProgress = (100 - (timeLeft / inocPeriod) * 100).toFixed(2);

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
    {daysLabel}
  </p>
  <p id="hours">
    <span class="number">
      {hours}
    </span>
    {hoursLabel}
  </p>
  <p id="mins">
    <span class="number">
      {minutes}
    </span>
    {minutesLabel}
  </p>
  <p id="secs">
    <span class="number">
      {seconds}
    </span>
    {secondsLabel}
  </p>
  <div id="progress" aria-label="{inocWaitingProgress}% of the way there!">
    <div id="bar" style="--progress-width: {inocWaitingProgress}%">
      <span id="progess-label">{inocWaitingProgress}%</span>
    </div>
  </div>
  <p id="remain">
    until we're &nbsp;<span class="strike">free</span> &nbsp; inoculated!
  </p>
</main>

<style>
  main {
    border: 1rem solid;
    overflow: hidden;
    font-size: clamp(14px, 2vw, 2rem);
    min-height: 100vh;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: 2fr 1fr auto 0.75fr;
    grid-template-areas:
      "days days days"
      "hours mins secs"
      "progress progress progress"
      "remain remain remain";
  }

  p {
    padding: 1rem;
    border: 1px solid;
    margin: 0;
    display: grid;
    place-content: center;
    text-align: center;
    text-transform: uppercase;
    letter-spacing: 0.25ch;
    font-weight: 900;
    line-height: 1;
  }

  .strike {
    text-decoration: line-through;
  }

  .number {
    font-size: 300%;
    font-weight: 200;
    margin-bottom: 0.25rem;
    margin-top: -0.2em;
  }

  #progress {
    grid-area: progress;
    background: var(--text);
  }

  #bar {
    background: var(--background);
    width: var(--progress-width);
    margin: 0.75rem 0;
    display: flex;
    position: relative;
  }

  #bar::before,
  #bar::after {
    content: "";
    background: var(--background);
    height: 70%;
    width: 1em;
    position: absolute;
    transform-origin: top right;
    transform: rotate(-45deg);
    right: 0;
    top: 0;
  }
  #bar::after {
    transform-origin: bottom right;
    transform: rotate(45deg);
    top: initial;
    bottom: 0;
  }
  #progess-label {
    color: var(--text);
    width: 100%;
    padding: 0.5em 0;
    padding-right: 0.25em;
    text-align: right;
    height: 100%;
    font-weight: 900;
    line-height: 1;
    z-index: 1;
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
    font-weight: 800;
    display: flex;
    align-items: center;
  }

  @media (max-width: 600px) {
    main {
      min-height: 90vh;
      border: none;
      grid-template-rows: 1fr auto auto 1fr;
    }
    p {
      border: none;
    }

    #hours {
      border-top: 1px solid;
      border-bottom: 1px solid;
    }
    #mins {
      border: 1px solid;
    }
    #secs {
      border-top: 1px solid;
      border-bottom: 1px solid;
    }

    #remain {
      align-items: flex-start;
      padding-top: 2rem;
    }
  }
</style>
