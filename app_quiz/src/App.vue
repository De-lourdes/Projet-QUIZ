<template>
  <div class="container-app">
    <div class="container-quiz">
      <div class="quiz-header">
        <h1>Quiz App</h1>
      </div>
      <div class="quiz-main" v-for="(element,index) in questions.slice(a,b)" :key="index" v-show="quiz">
        <div class="box-question">
          <h1>Question {{b}}/{{questions.length}}</h1>
          <p>{{element.question}}</p>
        </div>
        <div class="box-suggestions">
          <ul>
            <li v-for="(item,index) in element.suggestions" :key="index" :class="select ? check(item) : ''" @click="SelectResponse(item)">{{item.suggestion}}</li>
          </ul>
        </div>
      </div>
      <div class="box-score" v-if="score_show">
        <h2>Votre score est de:</h2>
        <h2>{{score}}/{{questions.length}}</h2>
        <div class="btn-restart">
          <button @click="restartQuiz()">Recommencer</button>
        </div>
      </div>
      <div class="quiz-footer">
        <div class="box-button" v-if="!score_show">
          <button @click="skipQuestion()" :style="!next ? 'background-color:rgb(106,128,202)':''">Passer</button>
          <button @click="nextQuestion()" :style="next ? 'background-color:rgb(106,128,202)':''">Suivant</button>
        </div>
      </div>
    </div>     
  </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return{
      questions:[
        {
          question:'Quelle est la capitale de la France?',
          suggestions:[
            {suggestion:'Marseille'},
            {suggestion:'Lyon'},
            {suggestion:'Paris', correct:true},
            {suggestion:'Bordeaux'},
          ]
        },
        {
          question:'En quelle année avait débuté la deuxième guerre mondiale?',
          suggestions:[
            {suggestion:'1914'},
            {suggestion:'1939', correct:true},
            {suggestion:'1945'},
            {suggestion:'1959'},
          ]
        },
        {
          question:'L"union européenne est composée de combien de pays?',
          suggestions:[
            {suggestion:'18'},
            {suggestion:'38'},
            {suggestion:'8'},
            {suggestion:'28', correct:true},
          ]
        },
        {
          question:'En quelle année fut lancé la construction de la tour Eiffel?',
          suggestions:[
            {suggestion:'1887', correct:true},
            {suggestion:'1787'},
            {suggestion:'1987'},
            {suggestion:'1687'},
          ]
        },
      ],
      a:0,
      b:1,
      select:false,
      score:0,
      quiz:true,
      score_show:false,
      next:false,
    }
  },
  methods:{

    SelectResponse(e){
      this.select = true;
      this.next = true;
      if (e.correct){
        this.score++;
      }
    },

    check(status){
      if (status.correct){
        return 'correct'
      }else{
        return 'incorrect'
      }
    },
    nextQuestion(){
      if (!this.next) {
        return;
      }
      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
        
      }
      else{
        this.a++;
        this.b++;
        this.select = false;
        this.next = false;
      }
    },
    skipQuestion(){
      if (this.next) {
        return;
      }
      if (this.questions.length -1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      }
      else {
        this.a++;
        this.b++;
      }
    },
    restartQuiz() {
      Object.assign(this.$data, this.$options.data()); //fonction pour reset les data
    }

  }
}
</script>

<style>
* {
  font-family: 'Poppins', sans-serif;
  margin: 0;
  letter-spacing: 2px;
}
.container-app{
  display: flex;
  width: 100%;
  height: 100%;
  justify-content: center;
}
.container-quiz{
  display: flex;
  width: 40%;
  height: 85%;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto;
  flex-flow: column;
  text-align: center;
  border: 1px solid #e7eae0;
  border-radius: 10px;
  background-color: white;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.23);
}
.quiz-header{
  display: flex;
  width: 100%;
  height: 20%;
  border-bottom: 1px solid #e7eae0;
  justify-content: center;
  align-items: center;
  background-color: #e7eae0;
  border-radius: 10px 10px 0px 0px ;
}
.quiz-main{
  display: flex;
  width: 100%;
  height: 70%;
  flex-flow: column;
  margin: auto;
}
.quiz-footer{
  display: flex;
  width: 100%;
  height: 10%;
  justify-content: center;
  border-top: 1px solid #e7eae0;
  background-color: #e7eae0;
  border-radius: 0px 0px 10px 10px;
}
.box-question{
  margin-top: 20px;
}
.box-question p{
  margin-top: 20px;
}
.box-suggestions{
  display: flex;
  width: 100%;
  margin: auto;
  justify-content: center;
}
ul{
  display: flex;
  width: 80%;
  margin: 0;
  padding: 0;
  flex-flow: column;
}
ul li{
  list-style: none;
  line-height: 2;
  border: 1px solid #cad2d2;
  margin-bottom: 20px;
  border-radius: 15px;
  cursor: pointer;
  
}
ul li:hover{
  background-color: #e7eae0;
}
.box-button{
  display: flex;
  width: 100%;

}
.box-button button{
  width: 150px;
  height: 35px;
  outline: none;
  border: 0;
  color: white;
  font-size: 18px;
  cursor: pointer;
  border-radius: 15px;
  margin: auto;
  background-color: #a09f9ff5;
}
li.correct{
  border: 1px solid green;
  background-color: green;
  color: white;
  font-weight: 600;
}
li.correct:hover{
  background-color: green;
}
li.incorrect{
  border: 1px solid red;
  background-color: red;
  color: white;
  font-weight: 600;
}
li.incorrect:hover{
  background-color: red;
}
.box-score{
  display: flex;
  width: 100%;
  height: 70%;
  flex-flow: column;
}
.box-score h2{
  margin-top: 40px;
}
.btn-restart {
  display: flex;
  width: 100%;
  height: auto;
  justify-content: center;
  margin-top: 50px;
}
.btn-restart button {
  width: 250px;
  height: 35px;
  outline: none;
  border: 0;
  color: white;
  font-size: 18px;
  cursor: pointer;
  border-radius: 15px;
  margin: auto;
  background-color: rgb(106, 128, 202);
}
</style>
