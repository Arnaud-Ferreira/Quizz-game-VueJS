<template>

  <div>
    <!-- v-html to notice vue that is html -->
   <h1 v-html="this.question" />
    <div class="inputs">
   <input type="radio" name="options" value="True">
   <label>True</label><br>

   <input type="radio" name="options" value="False">
   <label>False</label><br><button class="send" type="button">Send</button>

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
      correctAnswer: []
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
