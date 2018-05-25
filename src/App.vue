<template>
   <div id="app" class="container" >
      <div class="row">
         <div class="col-sm-12">
            <app-config v-if="!started"
               @getQuestionsArr='getQuestionsArr($event)'></app-config>
            <br>
            <app-question
               v-if = "started"
               :question="question"
               :currentQuestion="currentQuestion"
               :maxQuestions="maxQuestions"
               :rightAnswers="rightAnswers"
               :wrongAnswers="wrongAnswers"
               :ended="ended"
               @clickAnswer = 'clickAnswer($event)'
               ></app-question>
            <app-results
               :myAnswers="myAnswers"
               :ended = "ended"
               @startAgain='startAgain($event)'
               ></app-results>
         </div>
      </div>
   </div>
</template>

<script>
import Config from './components/Config.vue'
import Question from './components/Question.vue'
import Results from './components/Results.vue'

export default {
   name: 'app',
   data() {
      return {
         started: false,
         ended: false,
         currentQuestion: 0,
         maxQuestions: 0,
         rightAnswers: 0,
         wrongAnswers: 0,
         randomizedArr: [],
         myAnswers: {
            rightSum: 0,
            wrongSum: 0,
            answerArr: []
         },
         question: {
            country: '',
            capital: '',
            fourCapitals: ''
         }
      }
   },
   components: {
      appConfig: Config,
      appQuestion: Question,
      appResults: Results
   },
   methods: {
      startAgain(poruka) {
         this.started = false;
         this.ended = false;
         this.currentQuestion = 0;
         this.maxQuestions = 0;
         this.rightAnswers = 0;
         this.wrongAnswers = 0;
         this.randomizedArr = [];
         this.myAnswers = {
            rightSum: 0,
            wrongSum: 0,
            answerArr: []
         };
         this.question = {
            country: '',
            capital: '',
            fourCapitals: ''
         };
         this.started = false;
         this.ended = false;
         this.currentQuestion = 0;
      },
      callResults() {
         this.ended = true;
      },
      clickAnswer(obj) {
         let answer = obj.ansClickedCity;
         let isAnswerTrue = obj.ansBool;
         let obj2 = {
            country: this.question.country,
            capital: this.question.capital,
            answer: answer,
            isAnswerTrue: isAnswerTrue
         }
         if (isAnswerTrue === true) {
            this.rightAnswers++;
            this.myAnswers.answerArr.push(obj2);
         } else {
            this.wrongAnswers++;
            this.myAnswers.answerArr.push(obj2);
         }
         this.nextQuestion();
      },
      getQuestionsArr(arr) { //get array with choosen questions from child component Config.vue
         this.choosenArr = arr.splice('');
         this.randomizedArr = this.randomize(this.choosenArr, 10); //uraditi da se inz configa namesta drugi argument - maksimum pitanja
         this.maxQuestions = this.randomizedArr.length;
         this.start();
      },
      start() {
         this.started = true;
         this.question = this.randomizedArr[this.currentQuestion];
         this.question.fourCapitals = this.buildFourCapitals(this.randomizedArr, this.question.capital);
         this.currentQuestion = 1;
      },
      nextQuestion() {
         let self = this;
         if (this.currentQuestion + 1 <= 10) { //15 zameniti za promenljivu - broj pitanja
            this.question = this.randomizedArr[this.currentQuestion];
            this.question.fourCapitals = this.buildFourCapitals(this.choosenArr, this.question.capital);
            this.currentQuestion++;
         } else {
            setTimeout(function() {
               self.callResults();
            }, 2000)
         }
      },
      randomize(arr, max) {
         let arrFinal = [];
         let arrCopy = [];
         let len = max || arr.length; // || staviti mozda svuda gde idu argumenti da bi bio zasticen
         arrCopy = arr.slice('');
         for (var i = 0; i < len; i++) {
            let rand = Math.floor(Math.random() * arrCopy.length);
            arrFinal.push(arrCopy[rand]);
            arrCopy.splice(rand, 1);
         }
         return arrFinal;
      },
      buildFourCapitals(arr, trueAns) {
         let arrFour = [];
         let arrCopy = [];
         let len = 3;
         arrCopy = arr.slice('');
         for (var i = 0; i < arr.length; i++) {
            if (arr[i].capital === trueAns) {
               arrCopy.splice(i, 1);
            }
         }
         for (var i = 0; i < len; i++) {
            let rand = Math.floor(Math.random() * arrCopy.length);
            arrFour.push(arrCopy[rand].capital);
            arrCopy.splice(rand, 1);
         }
         arrFour.push(trueAns);
         return this.randomize(arrFour);
      }
   }
}
</script>

<style>
#app {
   font-family: 'Avenir', Helvetica, Arial, sans-serif;
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
   text-align: center;
   color: #2c3e50;
   margin-top: 60px;
   user-select: none;
   box-shadow: 0px 0px 10px black;
   background-color: #00a8ff;
   opacity: 0.97;
   max-width: 1000px;
}

html {
   background: url(src\assets\cont_imgs\back.jpg) no-repeat center center fixed;
   background-size: cover;
   height: 100%;
}

body{
   background-color: transparent;
}

@media screen and (max-width: 768px) {
   #app {
      margin-top: 0px;
   }
}

</style>
