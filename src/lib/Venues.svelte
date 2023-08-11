<!-- src/lib/Venues.svelte -->
<script>
  import { onMount } from 'svelte';

  let venues = [];
  let selectedVenues = [];
  let searchQuery = '';

  onMount(loadVenues);

  async function loadVenues() {
    const url = `https://maps.googleapis.com/maps/api/place/nearbysearch/json?location=38.2527,-85.7585&radius=5000&type=restaurant,bar&key=YOUR_API_KEY`;
    const response = await fetch(url);
    const data = await response.json();
    venues = data.results;
  }

  function selectVenue(venue) {
    selectedVenues.push(venue);
  }

  function searchVenues() {
    // Filter venues based on search query
    venues = venues.filter((venue) => venue.name.toLowerCase().includes(searchQuery.toLowerCase()));
  }
</script>

<input type="text" bind:value={searchQuery} placeholder="Search venues..." />
<button on:click={searchVenues}>Search</button>

<ul>
  {#each venues as venue}
    <li>
      <button on:click={() => selectVenue(venue)}>{venue.name}</button>
      <p>{venue.vicinity}</p>
    </li>
  {/each}
</ul>

<h3>Selected Favorites:</h3>
<ul>
  {#each selectedVenues as venue}
    <li>{venue.name}</li>
  {/each}
</ul>
