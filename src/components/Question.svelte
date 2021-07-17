<script>
  import Button from "../shared/Button.svelte";

  import Card from "../shared/Card.svelte";
  export let questions;
  const shuffle = (array) => {
    return array.sort(() => Math.random() - 0.5);
  };
  let disabled = true;

  let activeQuestion = questions[0];
  let incorrect_answers = questions[0]["incorrect_answers"];
  let correct_answer = questions[0]["correct_answer"];
  let answers = [...incorrect_answers, correct_answer];
  answers = shuffle(answers);
  $: currentIndex = 0;
  $: score = 0;
  $: active = false;
  $: disableAnswerButton = false;
  $: cursor = true;
  $: totQuestion = questions.length;
  $: wrong = false;
  $: correct = false;
  const nextQuestion = () => {
    if (currentIndex !== totQuestion - 1) {
      currentIndex++;
      activeQuestion = questions[currentIndex];
      incorrect_answers = questions[currentIndex]["incorrect_answers"];
      correct_answer = questions[currentIndex]["correct_answer"];
      answers = [...incorrect_answers, correct_answer];
      answers = shuffle(answers);
    } else {
    }
    correct = false;
    wrong = false;
    disabled = !disabled;
    active = !active;
    disableAnswerButton = !disableAnswerButton;
  };
  const handleAnswer = (answer) => {
    if (!disableAnswerButton) {
      if (answer === correct_answer) {
        score++;
        correct = true;
      } else {
        wrong = true;
      }
      disableAnswerButton = !disableAnswerButton;
      disabled = !disabled;
      active = !active;
    }
  };
</script>

<div class="questions">
  <div class="top">
    <div class="title">
      <Card active>Question {currentIndex + 1} of {totQuestion}</Card>

      <Card>Score: {score}</Card>
    </div>
    <div class="question-title">
      <Card>
        <p>{activeQuestion["question"]}</p>
      </Card>
    </div>
  </div>
  {#if disableAnswerButton}
    <div class="feedback" class:wrong class:correct>
      {#if correct}
        Correct
      {:else}
        Incorrect
      {/if}
    </div>
  {/if}
  <div class="question-list">
    {#each answers as answer (Math.random() * 10000)}
      <div class="question" on:click|once={() => handleAnswer(answer)}>
        <Card {cursor}>
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
  .wrong {
    color: red;
    padding: 0.8em 1em;
    /* background-color: rgba(0, 0, 0, 0.185); */
    /* border-bottom: 2px solid red; */
  }
  .feedback {
    margin-top: 1em;
  }
  .correct {
    color: green;
    padding: 0.8em 1em;
    /* background-color: rgba(0, 0, 0, 0.185); */
    /* border-bottom: 2px solid green; */
  }
  .question-title {
    align-self: stretch;
  }
  .top:first-child {
  }
</style>
