<template>
  <div id="app">
    <ScoreBoard :winCount="win_count" :loseCount="lose_count" />

    <template v-if="question">
      <h1 v-html="question"></h1>

      <template v-for="(answer, index) in answers" :key="index">
        <input :disabled="answerSubmitted" type="radio" name="options" :value="answer" v-model="chosen_answer">
        <label v-html="answer"></label><br>
      </template>

      <button v-if="!answerSubmitted" @click="submitAnswer" class="send" type="button">Send</button>
    </template>

    <section class="result" v-if="answerSubmitted">
      <template v-if="chosen_answer === correctAnswer">
        <h4>&#9989; Congratulations, the answer "{{ correctAnswer }}" is correct.</h4>
      </template>
      <template v-else>
        <h4>&#10060; I'm sorry, you picked the wrong answer. The correct answer is "{{ correctAnswer }}".</h4>
      </template>
      <button @click="getNewQuestion" class="send" type="button">Next question</button>
    </section>
  </div>
</template>

<script>
import axios from 'axios';
import ScoreBoard from '@/components/Scoreboard.vue';

export default {
  name: 'App',
  components: {
    ScoreBoard
  },
  data() {
    return {
      chosen_answer: undefined,
      question: undefined,
      incorrectAnswers: [],
      correctAnswer: '',
      win_count: 0,
      lose_count: 0,
      answerSubmitted: false
    };
  },
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
      return answers;
    }
  },
  methods: {
    submitAnswer() {
      if (!this.chosen_answer) {
        alert('Pick one of the options');
      } else {
        this.answerSubmitted = true;
        if (this.chosen_answer === this.correctAnswer) {
          this.win_count++;
        } else {
          this.lose_count++;
        }
      }
    },
    async getNewQuestion() {
      this.chosen_answer = undefined;
      this.answerSubmitted = false;
      this.question = undefined;

      try {
        const response = await axios.get('https://opentdb.com/api.php?amount=1&category=30');

        if (response.status === 429) {
          alert('Too many requests. Please wait a moment before trying again.');
          return; // Evita fazer a requisição novamente imediatamente
        }

        this.question = response.data.results[0].question;
        this.incorrectAnswers = response.data.results[0].incorrect_answers;
        this.correctAnswer = response.data.results[0].correct_answer;
      } catch (error) {
        console.error("Erro ao buscar a questão:", error);
        alert('Desculpe, ocorreu um erro ao carregar a próxima pergunta. Tente novamente mais tarde.');
      }
    }
  },
  created() {
    this.getNewQuestion();
  }
};
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
}

h1 {
  margin-top: 40px;
}

input[type='radio'] {
  margin: 12px 4px;
}

button.send {
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #1867c0;
  border: 1px solid #1867c0;
  cursor: pointer;
}

section.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;

  span {
    padding: 8px;
    font-weight: bold;
    border: 1px solid black;
  }
}
</style>
