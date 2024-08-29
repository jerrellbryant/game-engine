<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  
  const dispatch = createEventDispatcher();
  let searchQuery = '';

  function handleSubmit() {
    dispatch('search', searchQuery);
  }
</script>

<form on:submit|preventDefault={handleSubmit} class="w-full max-w-md mx-auto">
  <div class="flex items-center border-b border-white py-2">
    <input 
      class="appearance-none bg-transparent border-none w-full text-white mr-3 py-1 px-2 leading-tight focus:outline-none"
      type="text" 
      placeholder="Search for games..." 
      bind:value={searchQuery}
    >
    <button 
      class="flex-shrink-0 bg-white hover:bg-gray-200 text-gray-800 font-bold py-2 px-4 rounded"
      type="submit"
    >
      Search
    </button>
  </div>
</form>