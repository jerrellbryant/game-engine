<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { gsap } from 'gsap';
  import { onMount } from 'svelte';

  interface Game {
  id: string;
  name: string;
  background_image: string;
  released: string;
  clip?: {
    clip: string;
  };
}

  export let games: Game[] = [];


   const dispatch = createEventDispatcher();

  function selectVideo(game:Game) {
    if (game.clip && game.clip.clip) {

      dispatch('selectVideo', {url: game.clip.clip } );
    }
  }

  onMount(() => {
    gsap.from('.game-item', { 
      opacity: 0, 
      y: 20, 
      stagger: 0.1, 
      duration: 0.5 
    });
  });
</script>

<div class="game-list">
    {#each games as game (game.id)}
      <div class="game-item bg-gray-800 p-4 rounded-lg mb-4">
        <img src={game.background_image} alt={game.name} class="w-full object-cover rounded mb-2">
        <h2 class="text-xl font-bold">{game.name}</h2>
        <p class="text-gray-400">Release Date: {game.released}</p>
        {#if game.clip}
          <button 
            class="mt-2 bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded"
            on:click={() => selectVideo(game)}
          >
            Watch Trailer
          </button>
        {:else}
          <p class="mt-2 text-gray-400">No trailer available</p>
        {/if}
      </div>
    {/each}
    
    {#if games.length === 0}
      <p>No games found. Try another search.</p>
    {/if}
</div>
