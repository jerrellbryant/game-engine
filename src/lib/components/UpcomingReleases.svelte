<script lang="ts">
import { onMount } from 'svelte';


  export let games: {
    id: string;
    name: string;
    background_image: string;
    released: string;
  }[] = [];

  onMount(async () => {
    const { gsap } = await import('gsap');
    const { ScrollTrigger } = await import('gsap/ScrollTrigger');
    gsap.registerPlugin(ScrollTrigger);
    gsap.from('.upcoming-game', { 
      opacity: 0, 
      x: -20, 
      stagger: 0.1, 
      duration: 0.5,
      scrollTrigger: {
        trigger: '.upcoming-releases',
        start: 'top bottom-=100px',
        toggleActions: 'play none none reverse'
      }
    });
  });
</script>

<div class="upcoming-releases mt-12">
  <h2 class="text-2xl font-bold mb-4">Upcoming Releases</h2>
  {#if games.length === 0}
    <p>No upcoming games found.</p>
  {:else}
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
      {#each games as game (game.id)}
        <div class="upcoming-game bg-gray-800 p-4 rounded-lg">
          <img src={game.background_image} alt={game.name} class="w-full h-32 object-cover rounded mb-2">
          <h3 class="text-lg font-semibold">{game.name}</h3>
          <p class="text-gray-400">Release Date: {game.released}</p>
        </div>
      {/each}
    </div>
  {/if}
</div>
