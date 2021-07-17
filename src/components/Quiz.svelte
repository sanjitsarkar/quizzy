<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import Button from "../shared/Button.svelte";
  import Question from "./Question.svelte";
  $: questionNumber = 0;
  let isStarted = false;
  let quiz = fetchQuiz();
  async function fetchQuiz() {
    const res = await fetch("https://opentdb.com/api.php?amount=10");
    const quiz = await res.json();
    return quiz;
  }

  function nextQuestion() {
    questionNumberr++;
  }
  function startQuiz() {
    questionNumber = 0;
    isStarted = !isStarted;
    quiz = fetchQuiz();
  }
</script>

<div class="quiz">
  {#if !isStarted}
    <h1>Quizzy</h1>
    <Button
      active="true"
      on:click={() => {
        startQuiz();
      }}>Start</Button
    >
  {:else}
    {#await quiz}
      <h4>Loading...</h4>
    {:then data}
      <Question questions={data.results} />
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
