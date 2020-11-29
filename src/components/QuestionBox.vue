<template>
  <div>
    <b-jumbotron>
      <template>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers" 
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ answer }}        
        </b-list-group-item>
      </b-list-group>

      <b-button 
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
      Submit
      </b-button>
      <b-button @click="next" variant="success" href="#">
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)    
      return answers
    }
  },
  data: function() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  methods: {
    selectAnswer(index){
      this.selectedIndex = index
    },
    submitAnswer(){
      let isCorrect = false

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index){
      let answerClass = ''

      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected'
      }
      else if (this.answered && this.correctIndex === index){
        answerClass = 'correct'
      }
      else if (this.answered && this.selectedIndex === index && this.correctIndex != index){
        answerClass = 'incorrect'
      }
      return answerClass
    }
  },
  mounted() {
    this.shuffleAnswers()
  },
  watch: {
    currentQuestion() {
      this.selectedIndex = null
      this.answered = false
      this.shuffleAnswers()
      }
  }
}
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #e6e6e6;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background-color: rgb(151, 151, 235);
}

.correct {
  background-color: rgb(113, 240, 113);
}

.incorrect {
  background-color: rgb(248, 141, 141);
}
</style>