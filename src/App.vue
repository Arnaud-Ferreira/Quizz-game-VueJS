<template>

  <div>

    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount" />

    <!-- if we got the questions we can show the answers -->
    <div class="question" v-if="this.question">

    <!-- v-html to notice vue that is html -->
   <h1 v-html="this.question" />

   <div v-bind:key="index" v-for="(answer,index) in this.answers" class="inputs">
    <input
      :disabled="this.answerSubmitted"
      v-model="this.chosenAnswer"
      type="radio"
      name="options"
      :value="answer"
      >

    <label v-html="answer"></label><br>

   </div>
    <!-- if the answer has not been submitted then we will display this button -->
    <button v-if="!this.answerSubmitted" @click="this.submitAnswer()" class="send" type="button">Send</button>

  </div>

  </div>

 <!-- When we submit the answer, the rest is shown -->
  <section v-if="this.answerSubmitted" class="result">
    <h4 v-if="this.chosenAnswer == this.correctAnswer"
    v-html="'✔️ Congratulations, the answer ' + this.correctAnswer + '  is correct'"
    >
    </h4>
    <h4 v-else
    v-html="'❌ I`m sorry, you picked the wrong answer. The correct is ' +  this.correctAnswer">
    </h4>
    <button @click="this.getNewQuestion()" class="send" type="button">Next question</button>
  </section>

</template>

<script>
  import ScoreBoard from './components/ScoreBoard.vue';

export default {

  name: 'App',
  components: {
   ScoreBoard
  },
  
  data() {
    return {
      question: undefined,
      incorrectAnswers: [],
      correctAnswer: [],
      chosenAnswer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0,
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

  methods: {
    submitAnswer: function() {
      if (!this.chosenAnswer) {
        alert('Pick one of the options');
      } else {
        this.answerSubmitted = true;
      if(this.chosenAnswer == this.correctAnswer) {
          this.winCount++;
        } else {
          this.loseCount++;
        }
      }
    },

    getNewQuestion() {
      // to not see the answer elements anymore
      this.answerSubmitted = false;
      this.chosenAnswer = undefined;
      this.question = undefined;

      this.axios
      .get('https://opentdb.com/api.php?amount=50')
      .then((response) => {
        this.question = response.data.results[0].question;
        this.incorrectAnswers = response.data.results[0].incorrect_answers;
        this.correctAnswer = response.data.results[0].correct_answer;
     });
    }

  },

  created() {
    this.getNewQuestion();
  }
}

</script>

<style>
body {
  background-color: #e6ecf3bf;
}
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
  padding: 0 20px;
  color: #fff;
  background-color: #3e7ec7;
  border: 1px solid #3e7ec7;
  cursor: pointer;
  border-radius: 3px;
}

.question {
  margin-top: 3rem;
}

h1 {
  margin-bottom: 2.5rem;
}

.inputs {
  margin-bottom: 1.5rem;
  font-size: 1rem;
}
</style>
