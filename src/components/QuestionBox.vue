<template>
  <div>
  <b-jumbotron>
    <template v-slot:lead>
      {{ currentQuestion.question}}
    </template>

    <hr class="my-4">
      <b-list-group>
        <b-list-group-item 
        v-for="(answer, index) in shuffledAnswers" 
        :key="index"
        @click="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected' : '']"
        >
          {{ answer }}
          </b-list-group-item>
      </b-list-group>

    <b-button variant="primary" 
      @click="submitAnswer"
    >Submit</b-button>
    <b-button variant="success" @click="next">Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>
  import _ from "lodash"

  export default {
    props: {
      currentQuestion: Object,
      next: Function,
      increment: Function
    },
    data() {
      return {
        selectedIndex: null,
        shuffledAnswers: []
      }
    },
    computed: {
      answers() {
        let answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)
        return answers
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler() {
          this.selected = null;
          this.shuffleAnswers()
        }
      }
    },
    methods: {
      selectAnswer(index) {
        return this.selectedIndex = index
      },
      submitAnswer() {
        let isCorrect = false

        if(this.selectedIndex === this.corretIndex) {
          isCorrect = true
        }

        this.increment(isCorrect)
      },
      shuffleAnswers() {
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
      }
    }
  }
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px;
    cursor: pointer;
  }

  .list-group-item:hover {
    background-color: #ccc;
  }

  .selected {
    background-color: rgb(115, 209, 233);
  }

  .correct {
    background-color: rgb(123, 207, 127);
  } 

  .incorrect {
    background-color: rgb(216, 105, 105);
  }

  .btn {
    margin: 0 5px;
  }


</style>