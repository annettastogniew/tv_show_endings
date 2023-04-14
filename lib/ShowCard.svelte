<script>
  export let show;
  export let showDetails;
  export let tabIndex;
</script>

<div class="show-card" role="button" on:click={() => showDetails(show)} tabindex={tabIndex} on:keydown={() => showDetails(show)}>
  <img class="show-img" src={"src/assets/" + show.pic} alt={show.name + " title card"} />
</div>

<style>
  .show-card {
    cursor: pointer;
  }
  
  .show-img {
    width: 10vw;
  }
</style>