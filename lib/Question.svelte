<script>
  import { createEventDispatcher } from "svelte";

  export let questionText;
  export let options;
  export let group;

  const dispatch = createEventDispatcher();

  const logChoice = () => {
    dispatch('logChoice', {
			text: group
		});
  }
</script>

<div>
  <h2>{questionText}</h2>
  <div class="options-container">
    {#each options as option}
      <label>
        <input type=radio bind:group={group} name={group} value={Object.keys(option)[0]} on:click={logChoice}>
        {Object.values(option)[0]}
      </label>
    {/each}
  </div>
</div>

<style>
  .options-container {
    display: flex;
    justify-content: space-evenly;
    margin: 1em 0;
  }

  label {
    font-size: 1.35em;
  }
</style>
