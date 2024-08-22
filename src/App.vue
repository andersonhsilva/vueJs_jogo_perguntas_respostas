<template>

  <ScoreBoard :userScore="this.userScore" :computerScore="this.computerScore" />

  <!-- Questão e Respostas -->
  <h1>Responda a Questão:</h1>
  <template v-if="this.question">
    <h2 v-html="this.question"></h2>

    <div class="answers">
      <div v-for="(answer, index) in this.answers" :key="index" class="answer-item">
        <input type="radio" name="options" :value="answer" :id="'option-' + index" v-model="this.chosen_answer"
          :disabled="this.answerSubmit" />
        <label :for="'option-' + index" v-html="answer"></label>
      </div>
    </div>

    <button class="send" type="button" @click="this.submitAnswer();" :disabled="this.answerSubmit">Enviar
      Resposta</button>
    <button class="send" type="button" @click="this.getNewQuestion();" style="background-color: grey;">Próxima
      Pergunta</button>
  </template>
</template>

<script>
import ScoreBoard from "@/components/ScoreBoard.vue"
export default {

  name: 'App',
  components: {
    ScoreBoard
  },

  data() {
    return {
      question: undefined,
      incorrect_answers: [],
      correct_answer: undefined,
      chosen_answer: undefined,
      userScore: 0,
      computerScore: 0,
      answerSubmit: false,
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
    },

    // envia a resposta do usuario
    submitAnswer() {
      if (this.chosen_answer) {

        this.answerSubmit = true; // bloqueia os botoes de escolha das respostas apos envio da resposta

        if (this.chosen_answer == this.correct_answer)
          this.userScore++;
        else
          this.computerScore++;

      } else {
        alert('Esolha uma das opções de respostas');
      }
    },

    // pega uma nova pergunta com respostas
    getNewQuestion() {

      this.answerSubmit = false;
      this.chosen_answer = undefined;
      this.question = undefined;

      var api = 'https://opentdb.com/api.php?amount=1&category=18';
      this.axios.get(api).then((response) => {

        let result = response.data.results[0];

        this.question = result.question;
        this.incorrect_answers = result.incorrect_answers;
        this.correct_answer = result.correct_answer;

        console.log(response.data)
      })
    },

  },

  created() {
    this.getNewQuestion();
  },

}
</script>

<style lang="scss">
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  color: #333;
  margin-top: 40px;
}

h1 {
  font-size: 28px;
  margin-bottom: 20px;
}

h2 {
  font-size: 22px;
  margin-bottom: 20px;
  color: #2c3e50;
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
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  transition: background-color 0.3s;
}

.answer-item:hover {
  background-color: #f0f0f0;
}

input[type="radio"] {
  margin-right: 10px;
}

label {
  font-size: 18px;
  color: #333;
}

button.send {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 18px;
  transition: background-color 0.3s;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  margin: 10px;
}

button.send:hover {
  background-color: #0056b3;
}
</style>
