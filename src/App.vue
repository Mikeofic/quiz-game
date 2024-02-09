<template>
  <div>
    <h1 v-html="this.question"></h1>

    <input type="radio" name="options" value="True" />
    <label>True</label><br />

    <input type="radio" name="options" value="False" />
    <label>False</label><br />

    <button class="send" type="button">Send</button>
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
