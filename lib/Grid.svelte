<script>
  export let showDetails;
  import Row from "./Row.svelte";
  export let shows;

  const showsByThrees = ogShows => {
    let newShows = [];
    for (let i = 0; i < ogShows.length; i++) {
        if (i % 3 === 0) {
          newShows.push([]);
        }
      newShows[newShows.length - 1].push(ogShows[i]);
    }
    return newShows;
  }

  const showRows = showsByThrees(shows);
</script>

<div class="grid-container">
  {#each showRows as showRow, i}
    <Row shows={showRow} rowIndex={i} showDetails={showDetails}/>
  {/each}
</div>

<style>
  .grid-container {
    max-width: 100%;
  }
</style>