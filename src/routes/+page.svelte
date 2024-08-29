<script lang="ts">
  import '../app.css'
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import SearchBar from '$lib/components/SearchBar.svelte';
  import GameList from '$lib/components/GameList.svelte';
  import VideoPlayer from '$lib/components/VideoPlayer.svelte';
  import UpcomingReleases from '$lib/components/UpcomingReleases.svelte';

  

  interface Game {
    id: string;
    name: string;
    background_image: string;
    released: string;
    clip?: {
      clip: string;
    };
  }

  interface UpcomingGame {
    id: string;
    title: string;
    releaseDate: string;
  }

  let searchResults: Game[] = [];
  let upcomingGames: Game[] = [];
  let selectedVideo: { url: string } | null = null;

  const apiKey = import.meta.env.VITE_RAWG_API_KEY;
  const apiUrl = 'https://api.rawg.io/api';

  onMount(() => {
    gsap.from('.app-container', { opacity: 0, y: 50, duration: 1 });
    fetchUpcomingGames();
  });

  async function handleSearch(event: CustomEvent<string>) {
    const query = event.detail;
    try {
        const response = await fetch(`${apiUrl}/games?key=${apiKey}&search=${query}`);
    if (!response.ok) {
        console.error(`Error: ${response.status} - ${response.statusText}`);
        return;
      }
      const data = await response.json();
      searchResults = data.results;
    } catch (error) {
      console.error('Error fetching search results:', error);
      searchResults = [];
    }
  }

  async function fetchUpcomingGames() {
    const today = new Date().toISOString().slice(0, 10);
    const nextYear = new Date(new Date().setFullYear(new Date().getFullYear() + 1)).toISOString().slice(0, 10);
    try {
    const response = await fetch(`${apiUrl}/games?key=${apiKey}&dates=2024-01-01,2024-12-31&ordering=-added`);      const data = await response.json();
      upcomingGames = data.results.slice(0, 10); // Limit to 10 games
    } catch (error) {
      console.error('Error fetching upcoming games:', error);
      upcomingGames = [];
    }
  }

  function handleVideoSelect(event: CustomEvent<Game>) {
    selectedVideo = event.detail.clip ? { url: event.detail.clip.clip } : null;
  }
</script>

<main class="app-container min-h-screen bg-gray-900 text-white p-8">
  <h1 class="text-4xl font-bold mb-8">
    <a href="/">
      Video Game Explorer
    </a>
  </h1>
  
  <SearchBar on:search={handleSearch} />
  
  <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mt-8">
    <GameList games={searchResults} on:selectVideo={handleVideoSelect} />
    <VideoPlayer video={selectedVideo} />
  </div>
  
  <UpcomingReleases games={upcomingGames} />
</main>