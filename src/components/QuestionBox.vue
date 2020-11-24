<template>
  <div>
    <b-jumbotron>
      <template>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group 
        v-for="(answer, index) in answers" 
        :key="index"
        @click.prevent="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected' : '']"
      >
        <b-list-group-item>
          {{ answer }}        
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
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
    next: Function
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
      shuffledAnswers: []
    }
  },
  methods: {
    selectAnswer(index){
      this.selectedIndex = index
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
    }
  },
  mounted() {
    this.shuffleAnswers()
  },
  watch: {
    currentQuestion() {
      this.selectedIndex = null
      this.shuffleAnswers
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
  background-color: rgb(136, 136, 238);
}

.correct {
  background-color: rgb(113, 240, 113);
}

.incorrect {
  background-color: rgb(248, 141, 141);
}
</style>