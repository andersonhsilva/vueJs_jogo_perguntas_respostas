<template>
    <h1>Responda a Questão:</h1>
    <template v-if="this.question">
      <h2 v-html="this.question"></h2>

      <div class="answers">
        <div v-for="(answer, index) in this.answers" :key="index" class="answer-item">
          <input type="radio" name="options" :value="answer" :id="'option-' + index" />
          <label :for="'option-' + index" v-html="answer"></label>
        </div>
      </div>

      <button class="send" type="button">Enviar</button>
    </template>
</template>

<script>
export default {

  name: 'App',

  data() {
    return {
      question: undefined,
      incorrect_answers: [],
      correct_answer: undefined,
    }
  },

  computed: {
    answers() {
      let answers = [...this.incorrect_answers];
      answers.push(this.correct_answer);
      return this.shuffleArray(answers);
    }
  },

  methods: {
    // Função para embaralhar o array
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }
  },

  created() {
    var api = 'https://opentdb.com/api.php?amount=1&category=18';
    this.axios.get(api).then((response) => {

      let result = response.data.results[0];

      this.question = result.question;
      this.incorrect_answers = result.incorrect_answers;
      this.correct_answer = result.correct_answer;

      console.log(response.data)
    })
  },

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

input[type="radio"] {
  margin-right: 8px;
}

label {
  margin-right: 16px;
}

.answers {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.answer-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

button.send {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

button.send:hover {
  background-color: #0056b3;
}
</style>
