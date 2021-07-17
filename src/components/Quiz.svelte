<script>
  import Card from "../shared/Card.svelte";
  import Button from "../shared/Button.svelte";
  import Question from "./Question.svelte";
  import QuizStore from "../stores/QuizStore";
  import axios from "axios";
  import Result from "./Result.svelte";
  let elevated = true;
  const fetchQuiz = async () => {
    const results = await axios.get("https://opentdb.com/api.php?amount=10");
    return await results.data["results"];
  };

</script>

<div class="quiz">
  <!-- {#if !isStarted}
    <h1>Quizzy</h1>

    <Button on:click|once={() => startQuiz()} {active}>
      {#if loading}
        Loading...
      {:else if restartQuiz}
        <Result />
      {:else}
        Start Quiz
      {/if}
    </Button>
  {:else} -->
  <!-- {#await questions}
    Loading...
  {:then data} -->
  <!-- <h1>{data[0]["correct_answer"]}</h1> -->
  <Question />
  <!-- {/await} -->
  <!-- {/if} -->
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
