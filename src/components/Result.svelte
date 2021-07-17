<script>
  import Button from "../shared/Button.svelte";

  import Card from "../shared/Card.svelte";
  export let score;
  export let totQuestion;
</script>

<div class="result">
  <h1>Result</h1>
  <Card>
    <h3>You scored {score} out of {totQuestion}</h3>
  </Card>
  <Button>Start Again</Button>
</div>

<style>
  .result {
  }
</style>
