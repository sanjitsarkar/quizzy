<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import Button from "../shared/Button.svelte";
  import Question from "./Question.svelte";
  let isStarted = false;
  let questions = [];
  async function fetchQuiz() {
    const result = await fetch("https://opentdb.com/api.php?amount=10");
    const data = await result.json();
    return data.results;
  }

  function startQuiz() {
    isStarted = true;
    questions = fetchQuiz();
  }
</script>

<div class="quiz" transition:fade>
  {#if !isStarted}
    <h1>Quizzy</h1>
    <Button
      active="true"
      on:click={() => {
        startQuiz();
      }}>Start</Button
    >
  {:else}
    {#await questions}
      <h2>Loading...</h2>
    {:then questions}
      <Question {questions} on:startAgain={() => startQuiz()} />
    {/await}
  {/if}
</div>

<style>
  h1 {
    font-size: 5rem;
  }
  .quiz {
    min-height: 100vh;
    display: grid;
    place-content: center;
    place-items: center;
  }
</style>
