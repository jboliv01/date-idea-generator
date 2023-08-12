<script>
    import { onMount } from 'svelte';
  
    let venues = [];
    let selectedVenues = [];
    let searchQuery = '';
    let filteredVenues = [];
  
    onMount(loadVenues);
  
    async function loadVenues() {
      const map = new google.maps.Map(document.createElement('div'), {
        center: { lat: 38.2527, lng: -85.7585 },
        zoom: 15,
      });
  
      const request = {
        location: { lat: 38.2527, lng: -85.7585 },
        radius: '5000',
        keyword: 'bar restaurant food',
      };
  
      const service = new google.maps.places.PlacesService(map);
      service.nearbySearch(request, (results, status) => {
        if (status === google.maps.places.PlacesServiceStatus.OK) {
          venues = results;
          filteredVenues = [...results];
        }
      });
    }
  
    function selectVenue(venue) {
      selectedVenues = [...selectedVenues, venue];
    }
  
    $: filteredVenues = venues.filter((venue) => venue.name.toLowerCase().includes(searchQuery.toLowerCase()));
    $: isSelected = (venue) => selectedVenues.includes(venue);
</script>

<div class="flex justify-center">
    <div class="w-full max-w-screen-lg">
      <div class="w-full text-center mb-4">
        <input type="text" bind:value={searchQuery} placeholder="Search venues..." class="p-2 bg-transparent text-center border rounded w-full" />
      </div>
      <div class="flex flex-wrap justify-between">
        <div class="w-2/3">
          <div class="max-h-60 overflow-hidden">
            <ul class="w-full">
              {#each filteredVenues as venue}
                <li class="w-full rounded-lg p-4 text-primary-600 hover:bg-slate-400 cursor-pointer" on:click={() => selectVenue(venue)}>
                  <span class="text-primary-600">{venue.name}</span>
                  <p class="text-sm text-gray-600">{venue.vicinity}</p>
                </li>
              {/each}
            </ul>
          </div>
        </div>
        <div class="w-1/3 pl-4">
          <h3 class="text-lg font-bold mb-2">Favorites:</h3>
          <ul class="w-96">
            {#each selectedVenues as venue}
              <li class="w-full rounded-lg bg-primary-100 p-4 text-primary-600">{venue.name}</li>
            {/each}
          </ul>
        </div>
      </div>
    </div>
  </div>
  