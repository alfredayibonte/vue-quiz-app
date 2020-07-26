<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
       {{currentQuestion.question}}
      </template>

      <hr class="my-4"/>

      <b-list-group
        v-for="(answer, index) in answers"
        :key="answer"
        @click="selectAnswer(index)"
        >
        <b-list-group-item :class="[!answered && (selectedIndex === index) && 'selected']">
          {{answer}}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        :disabled="(selectedIndex === null) || answered"
        @click="submitAnswer"
      >
        Submit
      </b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import { shuffle } from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      },
    },
  },
  computed: {
    answers() {
      const answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    shuffleAnswers() {
      const answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
    },
    mounted() {
      this.shuffleAnswers();
    },
  },
};
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px;
    cursor: pointer;
  }
  .list-group-item:hover {
    background: #EEE;
  }
  .btn {
    margin: 0 5px;
  }
  .selected {
    background: lightblue;
  }
  .correct {
    background-color: lightgreen;
  }
  .incorrect {
    background-color: red;
  }
</style>
