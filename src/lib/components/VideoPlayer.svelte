<script lang="ts">
  import {onMount} from 'svelte'

interface Video {
    url: string;
}

  export let video: Video | null = null;

  let errorMessage: string | null = null;

  onMount(()=> {
    console.log('VideoPlayer mounted, video: ', video)
  })

  $: console.log('video changed: ', video)
  errorMessage = null;
  function handleError() {
    errorMessage = "Failed to load video. Please try again.";
    console.log("Video loading failed: ", video?.url)
  }
</script>

<div class="video-player">
  {#if video && video.url}
  <div class="relative" style="padding-top: 56.25%">
    <iframe
      class="absolute top-0 left-0 w-full h-full"
      src={video.url}
      title="Game Trailer"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen
      on:error = {handleError}
    ></iframe>
  </div>
  {#if errorMessage}
    <p class="text-red-500 mt-2">{errorMessage}</p>
  {/if}
    {:else if video}
     <div class="bg-gray-800 h-64 flex items-center justify-center rounded-lg">
        <p class="text-gray-400">No video available for this game.</p>
      </div>
    {:else}
      <div class="bg-gray-800 h-64 flex items-center justify-center rounded-lg">
        <p class="text-gray-400">Select a game to watch its trailer</p>
      </div>
    {/if}
</div>