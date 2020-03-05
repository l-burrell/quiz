<template>
  <div id="app">
    <Header :currentIndex="getIndex" :getTotal="getTotalQuestions" :numCorrect="numCorrect"/>
    <QuestionBox v-if="questions.length" :currentIndex="getIndex" :getTotal="getTotalQuestions" :currentQuestion="questions[index]" :next="next" />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
    }
  },
  methods: {
    next: function() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
    },
  },
  computed: {
    getIndex: function() {
      return this.index;
    },
    getTotalQuestions: function() {
      return this.questions.length;
    },
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&difficulty=easy&type=multiple', {
      method: 'get'
    })
      .then((response) => {
       return (response.json())
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      })
  },
}
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
#app {
  font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}
</style>
