<template>
  <div>
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>

      <template v-for="answer in this.answers" :key="answer">
        <input
          type="radio"
          name="options"
          :value="answer"
          v-model="this.chosen_answer"
        />
        <label v-html="answer"></label><br />
      </template>

      <button class="send" type="button" @click="submitAnswer">Send</button>
    </template>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosen_answer: undefined,
    };
  },
  computed: {
    answers() {
      let answers = [...this.incorrectAnswers];
      let pos = Math.floor(Math.random() * answers.length);
      answers.splice(pos, 0, this.correctAnswer);
      return answers;
    },
  },
  methods: {
    submitAnswer() {
      if (!this.chosen_answer) {
        alert("Escolha uma das opções.");
      } else {
        if (this.chosen_answer == this.correctAnswer) {
          alert(
            `Parabéns, a resposta correta é: "${this.correctAnswer}". Você acertou!`
          );
        } else {
          alert(
            `Infelizmente você errou. A resposta correta é: "${this.correctAnswer}". Tente novamente!`
          );
        }
      }
    },
  },
  created() {
    this.axios
      .get("https://opentdb.com/api.php?amount=1&category=18")
      .then((response) => {
        this.question = response.data.results[0].question;
        this.incorrectAnswers = response.data.results[0].incorrect_answers;
        this.correctAnswer = response.data.results[0].correct_answer;
      })
      .catch((error) => {
        alert(
          "Não foi possível gerar a sua pergunta, atualize a página.",
          error
        );
      });
  },
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
  max-width: 690px;

  input[type="radio"] {
    margin: 12px 4px;
  }

  .send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
