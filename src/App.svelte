<script>
  import dayjs from "dayjs";

  const params = window.location.search
    .substring(1)
    .split("&")
    .reduce((acc, curr) => {
      const [key, value] = curr.split("=");
      switch (key) {
        case "h":
          acc.hour = parseInt(value);
          return acc;
        case "m":
          acc.minute = parseInt(value);
          return acc;
        case "t":
          acc.text = decodeURIComponent(value);
          return acc;
        default:
          return acc;
      }
    }, {});

  const startTime =
    (params.hour &&
      dayjs()
        .hour(params.hour)
        .minute(params.minute || 0)
        .second(0)) ||
    0;

  let timeLeft = "--:--:--";

  function updateTimeLeft() {
    const now = dayjs();
    const hours = startTime.diff(now, "h");
    const minutes = startTime.diff(now, "m") - hours * 60;
    const seconds = startTime.diff(now, "s") - minutes * 60 - hours * 3600;

    timeLeft = `${padLeft(hours)}:${padLeft(minutes)}:${padLeft(seconds)}`;
  }

  function padLeft(input) {
    const asString = input.toString();
    return `${"0".repeat(Math.max(2 - asString.length, 0))}${asString}`;
  }

  if (startTime) {
    updateTimeLeft();
    setInterval(updateTimeLeft, 1000);
  }
</script>

<style>
  :global(html),
  :global(body) {
    background-color: #334;
    color: #ddd;
    font-family: "JetBrains Mono", monospace;
    width: 100%;
    height: 100%;
  }

  :global(body) {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 5vw;
  }

  main {
    text-align: center;
  }

  h1,
  h4 {
    margin: 0;
    text-shadow: #000 -10px 20px;
  }

  h1 {
    font-size: 3em;
  }

  h4 {
    text-transform: uppercase;
  }
</style>

<main>
  {#if startTime}
    <h4>{params.text || 'Starting in'}</h4>
    <h1>{timeLeft}</h1>
  {:else}
    <h4>{params.text || 'Starting soon'}</h4>
  {/if}
</main>
