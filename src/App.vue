<template>
  <div id="app">
    <Header 
      :numCorrect = "numCorrect"
      :numTotal = "numTotal" 
    />
    <b-container class="bv-example-row">
      <b-row sm="6" offset="3">
        <b-col>
          <QuestionBox 
            v-if = "questions.length && !gotoResults"
            :currentQuestion = "questions[index]"
            :nextQuest = "next"
            :increment = "increment"
          />
        </b-col>
      </b-row>
      <b-row sm="6" offset="3">
        <b-col>
          <Results 
            v-if = "gotoResults"
            :numCorrect = "numCorrect"
            :numTotal = "numTotal"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import Results from './components/Results.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
    Results
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      gotoResults: false
    }
  },
  methods:{
    next: function() {
      if(this.index<9){
        this.index++
      }
      else{
        this.gotoResults =  true
      }
    },
    increment(isCorrect) {
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=11&type=multiple',{
      method: 'get'
    })
      .then((response) => {
        return response.json()
      })
      .then((jsonData) => {
        this.questions = jsonData.results
         
      })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
