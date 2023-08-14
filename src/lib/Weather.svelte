<!-- src/lib/Weather.svelte -->
<script>
  import { onMount } from 'svelte';
  
  const apiKey = import.meta.env.VITE_OPENWEATHERMAP_API_KEY;
  let weather = null;
  const lat = 38.2527;
  const lon = -85.7585;
  const excludeParts = 'minutely,hourly'; // Exclude parts as needed

  onMount(async () => {
    const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&exclude=${excludeParts}&appid=${apiKey}`;
    const response = await fetch(url);
    weather = await response.json();
  });
</script>

<div class="p-4 font-berkshire-swash text-center capitalize grid grid-cols-3 gap-3 text-slate-400">
  {#if weather}
    <h2 class="text-5xl">{weather.name}</h2>
    <h2 class="text-3xl">{(weather.main.temp - 273.15).toFixed(2)}°C / {((weather.main.temp - 273.15) * 9/5 + 32).toFixed(2)}°F</h2>
    <h2 class="text-5xl">{weather.weather[0].description}</h2>
  {/if}
</div>