<script>
  import Card from "../shared/Card.svelte";
  import Button from "../shared/Button.svelte";
  import Question from "./Question.svelte";
  import QuizStore from "../stores/QuizStore";
  import axios from "axios";
  import Result from "./Result.svelte";
  let elevated = true;
  $: isStarted = false;
  $: loading = false;
  $: questions = [];
  $: active = true;
  $: restartQuiz = false;
  const startQuize = () => {
    loading = true;
    QuizStore.update(async (quiz) => {
      const results = await axios.get("https://opentdb.com/api.php?amount=10");
      // console.log(results.data["results"]);
      loading = false;
      isStarted = !isStarted;
      questions = results.data["results"];
      return [results.data["results"], ...quiz];
    });
  };
  //   startQuiz();
</script>

<div class="quiz">
  {#if !isStarted}
    <h1>Quizzy</h1>

    <Button on:click|once={() => startQuize()} {active}>
      {#if loading}
        Loading...
      {:else if restartQuiz}
        <Result />
      {:else}
        Start Quiz
      {/if}
    </Button>
  {:else}
    <Question {questions} />
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
