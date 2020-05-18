<template>
  <div>
    <b-jumbotron>
      <template>{{ cQuestion.question }}</template>
      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer , i) in shuffledAnswers"
          :key="i"
          @click="changeIndex(i)"
          :class="correcFunct(i)"
        >{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        href="#"
        @click="submitAnswer"
        :disabled="cIndex ===null || answered"
      >submit</b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    cQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      cIndex: null,
      shuffledAnswers: [],
      correct_answer: null,
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [...this.cQuestion.incorrect_answers];
      answers.push(this.cQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    cQuestion: {
      immediate: true,
      handler() {
        this.cIndex = null;
        this.answered = null;
        this.correct_answer = null;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    changeIndex(i) {
      this.cIndex = i;
    },
    shuffleAnswers() {
      let answers = [
        ...this.cQuestion.incorrect_answers,
        this.cQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correct_answer = this.shuffledAnswers.indexOf(
        this.cQuestion.correct_answer
      );
    },
    submitAnswer() {
      let is_correct = false;
      if (this.cIndex === this.correct_answer) {
        is_correct = true;
      }
      this.answered = true;
      this.increment(is_correct);
    },
    correcFunct(index) {
      let answerClass = "";
      if (!this.answered && this.cIndex === index) {
        answerClass = "selected";
      } else if (this.answered && index === this.correct_answer) {
        answerClass = "correct";
      } else if (
        this.answered &&
        index === this.cIndex &&
        index !== this.correct_answer
      ) {
        answerClass = "incorrect";
      }
      return answerClass;
    }
  }
};
</script>

<style scoped>
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}

.btn {
  margin: 10px;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
</style>