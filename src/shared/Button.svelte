<script>
  export let elevated;
  export let active = false;
  export let disabled;
</script>

<button class="button" class:elevated class:active on:click {disabled}>
  <slot />
</button>

<style>
  .button {
    padding: 0.5rem 2rem;
    border: none;

    border-radius: 2em;
    background-color: white;
    color: darkturquoise;
    cursor: pointer;
    border: 1px solid darkturquoise;
  }
  .active {
    transition: all 0.3s ease-in-out;
    background-color: darkturquoise;
    color: white;
  }
  .elevated {
    box-shadow: 0 0 1em rgba(0, 0, 0, 0.103);
  }
</style>
