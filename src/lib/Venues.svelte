<script>
    import { Loader } from '@googlemaps/js-api-loader';
    import { onMount } from 'svelte';
  
    let venues = [];
    let selectedVenues = [];
    let searchQuery = '';
    let filteredVenues = [];
 
    const types = [
      "bar",
      "cafe",
      "restaurant",
      "food",
      "night_club",
      "movie_theater",
      "museum",
      "art_gallery",
      "park",
      "bowling_alley",
      "aquarium",
      "zoo",
      "spa",
      "point_of_interest"
    ];
    const location = { lat: 38.2527, lng: -85.7585 };
    const radius = '100000';
  
    onMount(async () => {
      const loader = new Loader({
        apiKey: import.meta.env.VITE_GOOGLE_PLACES_KEY,
        version: 'weekly',
        libraries: ['places'],
      });
  
      await loader.load();
  
      const map = new google.maps.Map(document.createElement('div'), {
        center: location,
        zoom: 15,
      });
  
      const service = new google.maps.places.PlacesService(map);
  
      for (const type of types) {
        const request = {
          location: location,
          radius: radius,
          keyword: type,
        };
  
        service.nearbySearch(request, (results, status, pagination) => {
          if (status === google.maps.places.PlacesServiceStatus.OK) {
            venues = [...venues, ...results];
            if (pagination.hasNextPage) {
              pagination.nextPage();
            }
          } else {
            console.error('Error fetching venues:', status);
          }
        });
      }
    });
  
    function selectVenue(venue) {
      selectedVenues = [...selectedVenues, venue];
    }
  
    function deleteVenue(venue) {
      if (confirm("Are you sure you want to delete this favorite?")) {
        selectedVenues = selectedVenues.filter((v) => v !== venue);
      }
    }
  
    $: filteredVenues = venues.filter((venue) => venue.name.toLowerCase().includes(searchQuery.toLowerCase()));
    $: isSelected = (venue) => selectedVenues.includes(venue);
  </script>
  
  <div class="flex justify-center">
    <div class="w-full max-w-screen-lg">
      <div class="w-full text-center mb-4">
        <input type="text" bind:value={searchQuery} placeholder="Search your favorite places.."
        class="w-10/12 p-3 font-berkshire-swash text-slate-900 text-center text-3xl rounded-full focus:outline-none focus:ring focus:ring-slate-300" />
      </div>
      <div class="w-10/12 max-h-40 overflow-scroll w-full mx-auto">
        <ul class="w-full mx-auto">
          {#each filteredVenues as venue}
          <li class="w-full rounded-lg p-4 text-primary-600 hover:bg-slate-700 cursor-pointer">
            <button on:click={() => selectVenue(venue)} class="w-full text-center" data-te-ripple-init>
              <span class="text-primary-600">{venue.name}</span>
              <p class="text-sm text-gray-600">{venue.vicinity}</p>
            </button>
          </li>
          {/each}
        </ul>
      </div>
      <div class="">
        <h3 class="text-lg font-bold mb-2 text-center border-3xl rounded-lg">Favorites</h3>
        <ul class="w-full">
          {#each selectedVenues as venue}
          <li class="w-full rounded-lg bg-primary-100 p-4 text-primary-600 hover:bg-red-600 cursor-pointer" on:click={() => deleteVenue(venue)}>
            <span class="text-primary-600">{venue.name}</span>
            <p class="text-sm text-gray-600">{venue.vicinity}</p>
          </li>
          {/each}
        </ul>
      </div>
    </div>
  </div>
  