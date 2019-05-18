<template>
  <div class="question-box-container">
    <b-jumbotron>

      <template slot="lead">
        {{question}}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for = "(answer,index) in shuffledAnswers"
          :key="index"
          @click = "selectAnswer(index)"
          :class = "answerClass(index)"
        >
          {{answer}}
        </b-list-group-item>
      </b-list-group>
      <b-button 
        variant="primary"
        @click = "submitAnswer" 
        :disabled= "selectedIndex === null || answered"
        >
        Submit
      </b-button>
      <b-button variant="success" @click = "nextQuest" :disabled = "!answered || index === 10">Next</b-button>
    </b-jumbotron>
</div>
</template>

<script>
  import _ from 'lodash'
  export default {
    props: {
      currentQuestion: Object,
      nextQuest: Function,
      increment: Function
    },
    data() {
      return{
        selectedIndex : null,
        correctIndex: null,
        shuffledAnswers: [],
        answered: false,
        index: 0
      }
    },
    computed: {
      question: function(){
        let question = this.currentQuestion.question
        return question.replace(/&quot;|&#039;/g,(matched) =>{
          if(matched === '&quot;')
            return '"'
          else {
            return '\''
          }
        })
      },
      answers: function() {
        let answrs = [...this.currentQuestion.incorrect_answers]
        answrs.push(this.currentQuestion.correct_answer)
        return answrs
      }
    },
    watch: {
      currentQuestion:{
        immediate: true,
        handler() {
          this.selectedIndex = null
          this.answered = false
          this.shuffleAnswers()
        }
      }
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
      },
      submitAnswer(){
        let isCorrect = false

        if(this.selectedIndex === this.correctIndex){
          isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
        this.index++
      },
      shuffleAnswers() {
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      },
      answerClass(index){
        if(!this.answered && this.selectedIndex === index){ 
          return 'selected'  
        }else if(this.answered && this.correctIndex === index){ 
          return 'correct' 
        }else if(
          this.answered &&
          this.selectedIndex === index && 
          this.correctIndex != index
        ){ 
           return 'incorrect' 
        }else{
          return ''
        }
      }
    }
  }
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px;
  }
  .list-group-item:hover {
    background: #efefef;
    cursor: pointer;
  }
  .btn {
    margin: 0 5px;
  }
  .selected{
    background: lightblue;
  }
  .correct {
    background: lightgreen;
  }
  .incorrect {
    background: red;
  }
</style>