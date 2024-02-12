<template>
  <div class="content">
    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount" />

    <template v-if="this.question">
      <h1 v-html="this.question"></h1>

      <div class="answer-options">
        <div v-for="answer in this.answers" :key="answer" class="answer-option">
          <input
            :disabled="this.answerSubmitted"
            type="radio"
            name="options"
            :value="answer"
            v-model="this.chosenAnswer"
            :id="'option-' + answer"
          />
          <label :for="'option-' + answer" v-html="answer"></label><br />
        </div>
      </div>

      <button
        class="send"
        type="button"
        @click="this.submitAnswer"
        v-if="!this.answerSubmitted"
      >
        Enviar resposta
      </button>

      <div class="result" v-if="this.answerSubmitted">
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
          Próxima pergunta
        </button>
      </div>
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
      winCount: 0,
      loseCount: 0,
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
          this.winCount++;
        } else {
          this.loseCount++;
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
body {
  background-color: #080a0b;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #e1dcd7;
  margin: 60px auto;
  max-width: 690px;
  box-sizing: border-box;

  .content {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 70vh;
  }

  input[type="radio"] {
    margin: 12px 4px;
  }

  .answer-option input[type="radio"] {
    display: none;
  }

  .answer-option label {
    display: inline-block;
    min-width: 100%;
    padding: 10px 0;
    background-color: #3498db;
    color: #fff;
    border-radius: 5px;
    cursor: pointer;
    margin: 5px;
    text-align: center;
    transition: all 0.3s ease;
  }

  .answer-option input[type="radio"]:checked + label {
    background-color: #29e0a9;
    outline: 1px solid white;
    padding: 12px 0;
  }

  .answer-option input[type="radio"]:hover + label {
    outline: 1px solid white;
  }

  .result h4 {
    padding: 16px;
    border-radius: 4px;
    color: black;
    background-color: rgb(255, 217, 39);
  }

  .send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    font-size: 16px;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;

    &:hover {
      background-color: #29e0a9;
    }
  }
}
</style>
