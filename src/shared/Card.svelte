<script>
  export let active;
  export let wrong;
  export let correct;
  export let cursor;
</script>

<div class="card" class:active class:wrong class:correct class:cursor>
  <slot />
</div>

<style>
  .cursor {
    cursor: pointer;
  }
  .card {
    padding: 1em 2em;
    background-color: white;
    border: 1px solid turquoise;
    box-shadow: 0 0 1em rgba(0, 0, 0, 0.103);
    border-radius: 1em;
  }
  .active {
    transition: all 0.3s ease-in-out;
    background-color: darkturquoise;
    color: white;
    border: 1px solid turquoise;
  }
  .wrong {
    border: 1px solid red;
    background-color: rgba(255, 0, 0, 0.1);
  }
  .correct {
    background-color: rgba(0, 128, 0, 0.1);
    color: darkturquoise;
    border: 1px solid green;
  }
</style>
