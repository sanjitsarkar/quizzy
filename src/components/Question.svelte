<script>
  import { onMount } from "svelte";

  import Button from "../shared/Button.svelte";
  import Card from "../shared/Card.svelte";
  import QuizStore from "../stores/QuizStore";
  // export let questions;
  $: questions = [];
  onMount(async () => {
    QuizStore.subscribe(async (data) => {
      questions = data;
      console.log(data);
    });
  });
  const shuffle = (array) => {
    return array.sort(() => Math.random() - 0.5);
  };
  $: count = 0;
  // let totQuestion = questions.length;
  let activeQuestion = questions[count];
  let loading = true;
  let disabled = true;
  let wrong = false;
  let correct = false;

  let incorrect_answers = questions[count]["incorrect_answers"];
  let correct_answer = questions[count]["correct_answer"];
  let answers = [...incorrect_answers, correct_answer];
  // answers = shuffle(answers);

  $: score = 0;
  $: active = false;
  $: disableButtonAnswer = false;
  $: cursor = true;

  const nextQuestion = () => {
    if (count !== totQuestion - 1) {
      count++;
    } else {
    }
    disabled = !disabled;
    active = !active;
    disableButtonAnswer = !disableButtonAnswer;
  };
  const handleAnswer = (answer) => {
    if (!disableButtonAnswer) {
      if (answer === correct_answer) {
        score++;
      } else {
      }
      disableButtonAnswer = !disableButtonAnswer;
      disabled = !disabled;
      active = !active;
    }
  };
</script>

<div class="questions">
  <div class="top">
    <div class="title">
      <Card active>Question {count + 1} of {questions.length}</Card>

      <Card>Score: {score}</Card>
    </div>
    <div class="question-title">
      <Card>
        <p>{activeQuestion["question"]}</p>
      </Card>
    </div>
  </div>
  <div class="question-list">
    {#each answers as answer (answer)}
      <div class="question" on:click|once={() => handleAnswer(answer)}>
        <Card {cursor} correct="true">
          <p>
            {answer}
          </p>
        </Card>
        <Card {cursor} wrong="true">
          <p>
            {answer}
          </p>
        </Card>
      </div>
    {/each}
  </div>
  <div class="bottom">
    <Button on:click={() => nextQuestion()} {disabled} {active}>Next</Button>
  </div>
</div>

<style>
  .question-list {
    width: 80vw;

    margin-top: 1em;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1em;
  }
  .title {
    display: flex;
    gap: 1em;
  }
  .bottom {
    margin-top: 1em;
    text-align: center;
  }
  .questions {
    /* width: 40vw; */
  }
  .question {
    transition: all 0.1s ease-in-out;
  }
  .question:hover,
  .question:focus {
    transition: all 0.1s ease-in-out;
    transform: scale(1.02);
  }
  .top {
    display: flex;
    gap: 1em;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
  }
  .score {
    width: auto;
  }
  .question-title {
    align-self: stretch;
  }
  .top:first-child {
  }
</style>
