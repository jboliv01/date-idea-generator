<!-- src/lib/Form.svelte -->
<script>
  import LoadingIndicator from './Loading.svelte';

  // List of favorite venues (restaurants, bars, etc.)
  let venues = [
    // Example: { name: 'Venue Name', type: 'Restaurant', walkingDistance: true }
  ];
  let selectedVenues = [];
  let walkingDistance = false;

  function selectVenue(venue) {
    selectedVenues.push(venue);
  }
</script>

<div class="pt-6 md:pt-10 text-slate-200">
  <div>
    <div class="mb-8">
      <div class="mb-4 font-semibold text-lg">Select your favorite venues:</div>
      <div class="flex items-center flex-wrap">
        {#each venues as venue}
          <button
            on:click={() => selectVenue(venue)}
            class="text-slate-200 font-bold mr-2 mt-2 text-sm py-2 px-4 rounded-full border border-pink-600"
          >
            {venue.name}
          </button>
        {/each}
      </div>
    </div>
    <div class="mt-8">
      <label>
        <input type="checkbox" bind:checked={walkingDistance} />
        Include only venues in walking distance?
      </label>
      <button
        on:click
        class="bg-pink-600 hover:bg-gradient-to-r from-pink-700 via-pink-600 to-pink-700 mt-4 w-full h-10 text-white font-bold p-3 rounded-full flex items-center justify-center"
      >
        <p>Generate Date Ideas</p>
      </button>
    </div>
  </div>
</div>
