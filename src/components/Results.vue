<template>
    <div>
        <b-jumbotron>
          <template slot="lead">
            <h1>Your score is: {{numCorrect}}/{{numTotal}}</h1>
            <br>
            <h1>{{description}}</h1>
          </template>
        </b-jumbotron>
    </div>
</template>

<script>
  export default {
    props: [
      'numCorrect',
      'numTotal'
    ],
    computed:{
      description: function(){
        if(this.numCorrect === 10){
          return 'Awesome!'
        }else if(this.numCorrect >= 7 ){
          return 'You \'re good!'
        }else if(this.numCorrect >=5){
          return 'You passed the test!'
        }else{
          return 'Failed!'
        }
        
      }

    }
  }
</script>