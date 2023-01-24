<template>
  <div>
    <ScoreBoard :computerScore="this.computerScore" :playerScore="this.playerScore"/>
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>

      <template v-for="(answer, idx) in this.answers" :key="idx">
        <input 
          type="radio" 
          name="options" 
          :value="answer"
          v-model="this.choosen"
          :disabled="this.answerSubmited"
         style="align:left"/>
        <label v-html="answer"></label><br/>
      </template>
      
      <button v-if="!this.answerSubmited" class="send" @click="this.submitChoosen()" type="button">Send</button>
      
      <section v-if="this.answerSubmited" class="result">
        <h4 v-if="this.choosen == this.wrightAnswer" >&#9989; Yaeh! Você escolheu a resposta certa!</h4>
        <h4 v-else 
          v-html="`&#10060; Ah não, você escolheu a resposta errada. A resposta correta é '${this.wrightAnswer}'`"></h4>
        <button class="send" @click="this.getNewQuestion()" type="button">Próxima pergunta</button>
      </section>

    </template>
  </div>
</template>

<script>

import ScoreBoard from '@/components/ScoreBoard.vue'

export default {
  name: 'App',

  data() {
    return {
      question : undefined,
      wrightAnswer : undefined,
      wrongAnswers : undefined,
      choosen : undefined,
      answerSubmited : false,
      computerScore : 0,
      playerScore : 0,

    }
  },
  computed: {
    answers(){
      var answers = JSON.parse( JSON.stringify(this.wrongAnswers) );
      
      answers.splice(Math.round(Math.random() * answers.length),0,this.wrightAnswer);
      return answers;
    }
  },
  methods: {
    submitChoosen(){
      if(!this.choosen){
        alert('É necessário escolher uma das opções!')
      }else{
        this.answerSubmited = true
        if(this.choosen === this.wrightAnswer){
          console.log('acertou')
          this.playerScore++
        }else{
          console.log('errou')
          this.computerScore++
        }
      }
    },
    getNewQuestion(){
      this.question = undefined,
      this.choosen = undefined,
      this.answerSubmited = false,
      this.axios
      .get(`https://opentdb.com/api.php?amount=1&category=18`)
      .then((res) => {
        this.question = res.data.results[0].question;
        this.wrongAnswers = res.data.results[0].incorrect_answers;
        this.wrightAnswer = res.data.results[0].correct_answer;
      })
    }
  },
  created() {
    this.getNewQuestion()
  },
  components: {
    ScoreBoard,
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px auto;
  max-width: 960px;

  input[type=radio]{
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
    border-radius: 5px;
  }
}
</style>
