<script>
  import Button from "../shared/Button.svelte";
  import Card from "../shared/Card.svelte";
  import QuizStore from "../stores/QuizStore";
  export let questions;
  // const shuffle = (array) => {
  //   return array.sort(() => Math.random() - 0.5);
  // };
  $: count = 0;
  $: totQuestion = questions.length;
  $: activeQuestion = questions[count];
  $: loading = true;
  $: disabled = true;
  $: wrong = false;
  $: correct = false;

  $: incorrect_answers = questions[count]["incorrect_answers"];
  $: correct_answer = questions[count]["correct_answer"];
  $: answers = [...incorrect_answers, correct_answer];
  // answers = answers.sort();

  $: score = 0;
  $: active = false;
  $: disableButtonAnswer = false;
  $: cursor = true;

  //   console.log(answers);
  const nextQuestion = () => {
    if (count !== totQuestion - 1) {
      count++;
      wrong = false;
      correct = false;
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
        {#if answer == correct_answer && !disableButtonAnswer}
          <Card {cursor} correct="true">
            <p>
              {answer}
            </p>
          </Card>
        {:else}
          <Card {cursor} {wrong}>
            <p>
              {answer}
            </p>
          </Card>
        {/if}
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
