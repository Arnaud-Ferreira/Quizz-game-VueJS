<template>

  <div>
    <!-- if we got the questions we can show the answers -->
    <div v-if="this.question">

    <!-- v-html to notice vue that is html -->
   <h1 v-html="this.question" />

   <div v-bind:key="index" v-for="(answer,index) in this.answers" class="inputs">
    <input
      type="radio"
      name="options"
      value="answer">

    <label v-html="answer"></label><br>

   </div>

    <button class="send" type="button">Send</button>

  </div>

  </div>
</template>

<script>

export default {
  name: 'App',
  
  data() {
    return {
      question: undefined,
      incorrectAnswers: [],
      correctAnswer: [],
    }
  },

  computed: {
    answers() {
    // we add the right and wrong answers in the same array
    // we need to parse it in JSON otherwise the values goes wrong
      const answers = JSON.parse( JSON.stringify(this.incorrectAnswers) );
    // To add the elements, we'll use the splice method
    // the first argument is the position of the element we want to add
    // the second is the number of elements we want to delete
    // the third is the element we want to add
    // In order to mix the array we'll use the math random method by multiplying by the number of elements in the array
      answers.splice( Math.round( Math.random() * answers.length) , 0, this.correctAnswer);
      return answers;
    // Now the right answer is always at a random position
    }
  },

  created() {
    this.axios
    .get('https://opentdb.com/api.php?amount=10&category=18')
    .then((response) => {
      this.question = response.data.results[0].question;
      this.incorrectAnswers = response.data.results[0].incorrect_answers;
      this.correctAnswer = response.data.results[0].correct_answer;
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;
}

#app input[type=radio] {
  margin: 12px 4px;
}

button.send {
  margin-top: 15px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #3e7ec7;
  border: 1px solid #3e7ec7;
  cursor: pointer;
  border-radius: 3px;
}
</style>
