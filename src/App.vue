<template>
  <div>
    <template v-if="this.question">

      <h1 v-html="this.question">
      </h1>
 
      <template v-for="(answer, index) in this.answers" v-bind:key="index"> 
        <input 
         :disabled="this.answerSubmitted"
         type="radio"
         name="options" 
         :value="answer"
         v-model="this.chosen_answers"> 
        <label v-html="answer"></label><br>
      </template>

      <button @click="this.submitAnswer()" class="send" type="button">Send</button>

    </template>

  </div>
</template>

<script>

export default {
  name: 'App',

  data(){
    return{
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosen_answers: undefined,
      answerSubmitted: false
    }
  },

  computed:{
    answers(){
      var answers = [...this.incorrectAnswers];
      answers.splice(Math.round(Math.random() * answers.length),0,this.correctAnswer);
      return answers;
    }
  },
  methods:{

    submitAnswer() {
      if (!this.chosen_answers) {
        alert('Pick one of the options');
      } else {
        this.answerSubmitted = true;
        if (this.chosen_answers == this.correctAnswer){
            console.log('You got it right!');
          } else {
            console.log('You got it wrong');
        }
      }
    }
  },
  created(){
    this.axios
    .get('https://opentdb.com/api.php?amount=1')
    .then((response) => {
      this.question = response.data.results[0].question;
      this.incorrectAnswers = response.data.results[0].incorrect_answers;
      this.correctAnswer = response.data.results[0].correct_answer;
    })
  }
}
//https://opentdb.com/api.php?amount=1
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type=radio]{
    margin: 12px 4px;
  }

  button.send{
    margin-top: 12px;
    padding: 0 16px;
    height: 40px;
    min-width: 120px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}

</style>
