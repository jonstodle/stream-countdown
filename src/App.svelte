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

  const startTime = dayjs().hour(params.hour).minute(params.minute).second(0);

  let timeLeft = "--:--:--";

  function updateTimeLeft() {
    const now = dayjs();
    const hours = startTime.diff(now, "h");
    const minutes = startTime.diff(now, "m");
    const seconds = startTime.diff(now, "s") - minutes * 60 - hours * 3600;

    timeLeft = `${hours}:${minutes}:${seconds}`;
  }

  updateTimeLeft();
  setInterval(updateTimeLeft, 1000);
</script>

<style>
  :global(html),
  :global(body) {
    background-color: white;
    font-family: "JetBrains Mono", monospace;
    width: 100%;
    height: 100%;
  }

  :global(body) {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 5vh;
  }

  main {
    text-align: center;
  }
</style>

<main>
  <h2>{params.text || 'Starting in'}</h2>
  <h1>{timeLeft}</h1>
</main>
