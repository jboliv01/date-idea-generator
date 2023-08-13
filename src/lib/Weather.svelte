<!-- src/lib/Weather.svelte -->
<script>
  import { onMount } from 'svelte';

  let weather = null;
  const lat = 38.2527;
  const lon = -85.7585;
  const excludeParts = 'minutely,hourly'; // Exclude parts as needed

  onMount(async () => {
    const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&exclude=${excludeParts}&appid=`;
    const response = await fetch(url);
    weather = await response.json();
  });
</script>

<div class="p-4 text-center text-3xl uppercase grid grid-cols-3 gap-4 bg-gradient-to-r from-red-600 via-slate-800 to-red-500 inline-block text-transparent bg-clip-text">
  {#if weather}
    <h2 class="text-3xl font-bold">{weather.name}</h2>
    <h2 class="text-3xl">{(weather.main.temp - 273.15).toFixed(2)}°C / {((weather.main.temp - 273.15) * 9/5 + 32).toFixed(2)}°F</h2>
    <h2 class="text-3xl">{weather.weather[0].description}</h2>
  {/if}
</div>