<script>
  import { onMount } from "svelte";
  import moment from "moment";
  let number = 0;
  const dollarsPerSecond = 328.1;
  const speed = 50; // milliseconds per number change
  const siteArrivalTime = moment().format();
  const date = {
    site: moment(siteArrivalTime),
    startofday: moment().startOf("day"),
    jan1: moment("2021-01-01"),
    covid: moment("2019-11-17"), // First case, Source: https://en.wikipedia.org/wiki/COVID-19_pandemic
    budget: moment("2021-05-11"), // Source: https://budget.gov.au/
    bushfires: moment("2019-09-06"), // Source: https://en.wikipedia.org/wiki/2019%E2%80%9320_Australian_bushfire_season#Domestic_response
  };
  const numOptions = {
    maximumFractionDigits: 0,
  };

  const handleChange = (event) => {
    const since = event.target.value;
    const now = moment();
    const difference = now.diff(date[since], "seconds");
    number = difference * dollarsPerSecond;
    console.log(number);
  };

  onMount(() => {
    console.log(date.startofday.format());
    console.log(date.jan1.format());
    setInterval(() => {
      number = number + dollarsPerSecond / (1000 / speed);
    }, speed);
  });
</script>

<div id="svelte" class="counter">
  <select on:change={handleChange} on:blur={handleChange}>
    <option value="site">Since coming to this site</option>
    <option value="startofday">So far today</option>
    <option value="jan1">Since Jan 1 this year</option>
    <option value="covid">Since the start of COVID</option>
    <option value="budget">Since the last federal budget</option>
    <option value="bushfires">Since the 2019 bushfires</option>
  </select>
  <div class="big-number">${number.toLocaleString(undefined, numOptions)}</div>
</div>

<style>
  select {
    font-family: inherit;
    background-color: black;
    color: white;
    font-size: 1.8rem;
    border: 0;
    border-bottom: 1px solid white;
    outline: none;
    margin: 1rem 0 3rem 0;
  }

  .big-number {
    font-size: 6rem; /* Fallback */
    font-size: clamp(2rem, 10vw, 8rem); /* Min, Variable, Max */
  }

  @media (max-width: 580px) {
    select {
      font-size: 1.2rem;
    }
  }
</style>
