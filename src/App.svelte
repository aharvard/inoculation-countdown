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
    minutes
  </p>
  <p id="secs">
    <span class="number">
      {seconds}
    </span>
    seconds
  </p>
  <p id="remain">
    until we're &nbsp;<span class="strike">free</span> &nbsp; inoculated!
  </p>
</main>

<style>
  main {
    border: 1rem solid;
    font-size: clamp(14px, 2vw, 2rem);
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
      border: none;
      grid-template-rows: 1fr auto 1fr;
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
