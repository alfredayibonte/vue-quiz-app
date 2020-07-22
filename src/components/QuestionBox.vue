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
        @click="selectAnswer(index)">
        <b-list-group-item>{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function,
  },
  data() {
    return {
      selectedIndex: null,
    };
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
  },
};
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px;
    cursor: pointer;
  }
  .btn {
    margin: 0 5px;
  }
</style>
