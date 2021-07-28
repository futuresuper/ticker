<script>
  import { onMount } from "svelte";
  import moment from "moment";
  let number = 0;
  let selected = "site";
  let showOptions = false;
  const dollarsPerSecond = 402;
  const speed = 50; // milliseconds per number change
  const siteArrivalTime = moment().format();
  const startTime = {
    site: moment(siteArrivalTime),
    startofday: moment().startOf("day"),
    oneJuly21: moment("2021-07-01"),
  };
  const dollarsAt30June21 = {
    jan1: 5991000000,
    covid: 19439380822,
    budget: 1641369863,
    bushfires: 21815183562,
  };
  const description = {
    site: "Since coming to this site",
    startofday: "So far today",
    jan1: "Since Jan 1 this year",
    covid: "Since the start of COVID",
    budget: "Since the last federal budget",
    bushfires: "Since the 2019 bushfires",
  };
  const numOptions = {
    maximumFractionDigits: 0,
  };

  const handleChange = (since) => {
    selected = since;
    const now = moment();
    if (since === "site" || since === "startofday") {
      const difference = now.diff(startTime[since], "seconds");
      number = difference * dollarsPerSecond;
    } else {
      number =
        dollarsAt30June21[since] + now.diff(startTime.oneJuly21, "seconds");
    }
  };

  const toggleShowOptions = () => {
    showOptions = !showOptions;
  };

  onMount(() => {
    // console.log(startTime.startofday.format());
    // console.log(startTime.jan1.format());
    setInterval(() => {
      number = number + dollarsPerSecond / (1000 / speed);
    }, speed);
  });
</script>

<div id="svelte" class="counter">
  <div id="select-container" on:click={toggleShowOptions}>
    <div>
      <div>{description[selected]}</div>
      {#if showOptions}
        <div id="other-options">
          {#if selected !== "site"}
            <div class="option" on:click={() => handleChange("site")}>
              {description.site}
            </div>
          {/if}
          {#if selected !== "startofday"}
            <div class="option" on:click={() => handleChange("startofday")}>
              {description.startofday}
            </div>
          {/if}
          {#if selected !== "jan1"}
            <div class="option" on:click={() => handleChange("jan1")}>
              {description.jan1}
            </div>
          {/if}
          {#if selected !== "budget"}
            <div class="option" on:click={() => handleChange("budget")}>
              {description.budget}
            </div>
          {/if}
          {#if selected !== "covid"}
            <div class="option" on:click={() => handleChange("covid")}>
              {description.covid}
            </div>
          {/if}
          {#if selected !== "bushfires"}
            <div class="option" on:click={() => handleChange("bushfires")}>
              {description.bushfires}
            </div>
          {/if}
        </div>
      {/if}
    </div>
    <svg
      width="39"
      height="23"
      viewBox="0 0 39 23"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
    >
      <path d="M2 2L19.5 19L37 2" stroke="white" stroke-width="5" />
    </svg>
  </div>
  <div class="big-number">${number.toLocaleString(undefined, numOptions)}</div>
</div>

<style>
  #select-container {
    font-size: 1.8rem;
    border: 0;
    border-bottom: 1px solid white;
    margin: 1rem 0 3rem 0;
    display: flex;
    justify-content: space-between;
    cursor: pointer;
  }

  #other-options {
    color: darkgray;
  }

  .option:hover {
    color: lightgrey;
  }

  svg {
    margin-left: 2rem;
  }

  .big-number {
    font-size: 6rem; /* Fallback */
    font-size: clamp(2rem, 10vw, 8rem); /* Min, Variable, Max */
  }

  @media (max-width: 580px) {
    #select-container {
      font-size: 1.2rem;
    }

    svg {
      width: 20px;
      margin-top: -2px;
    }
  }
</style>
