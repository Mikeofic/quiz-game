<template>
  <div>
    <ScoreBoard />

    <template v-if="this.question">
      <h1 v-html="this.question"></h1>

      <template v-for="answer in this.answers" :key="answer">
        <input
          :disabled="this.answerSubmitted"
          type="radio"
          name="options"
          :value="answer"
          v-model="this.chosenAnswer"
        />
        <label v-html="answer"></label><br />
      </template>

      <button
        class="send"
        type="button"
        @click="this.submitAnswer"
        v-if="!this.answerSubmitted"
      >
        Send
      </button>

      <section class="result" v-if="this.answerSubmitted">
        <h4
          v-if="this.chosenAnswer == this.correctAnswer"
          v-html="
            `&#9989; Congratulation, you picked the correct answer. The correct is: ${this.correctAnswer}`
          "
        ></h4>

        <h4
          v-else
          v-html="
            `&#10060; I'm sorry, you picked the wrong answer. The correct is: ${this.correctAnswer}`
          "
        ></h4>

        <button class="send" type="button" @click="this.getNewQuestion">
          Next question
        </button>
      </section>
    </template>
  </div>
</template>

<script>
import ScoreBoard from "./components/ScoreBoard.vue";

export default {
  name: "App",
  components: { ScoreBoard },
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
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
      if (!this.chosenAnswer) {
        alert("Escolha uma das opções.");
      } else {
        this.answerSubmitted = true;

        if (this.chosenAnswer == this.correctAnswer) {
          console.log(
            `Parabéns, a resposta correta é: "${this.correctAnswer}". Você acertou!`
          );
        } else {
          console.log(
            `Infelizmente você errou. A resposta correta é: "${this.correctAnswer}". Tente novamente!`
          );
        }
      }
    },

    getNewQuestion() {
      (this.answerSubmitted = false),
        (this.chosenAnswer = undefined),
        (this.question = undefined),
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
  },
  created() {
    this.getNewQuestion();
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
