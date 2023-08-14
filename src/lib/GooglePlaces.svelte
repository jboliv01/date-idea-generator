<script>
  import { writable } from 'svelte/store';

  export const googlePlacesAPIReady = writable(false);

  const GOOGLE_PLACES_KEY = import.meta.env.VITE_GOOGLE_PLACES_KEY;

  // Load the Google Places API script with the API key
  const script = document.createElement('script');
  script.src = `https://maps.googleapis.com/maps/api/js?key=${GOOGLE_PLACES_KEY}`;
  script.onload = () => googlePlacesAPIReady.set(true);
  document.head.appendChild(script);
</script>
