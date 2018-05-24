<template lang="html">
   <div class="row">
      <div class="col-sm-8 offset-sm-2 questionCont"
         v-if="!ended">
         <div class="row">
            <div class="col-sm-12 questionHead">
               <h3> {{ questionGen }} {{ question.country }} ?</h3>
            </div>
         </div>
         <div class="row">
            <div class="col-sm-12 col-md-6 col-pad"
               v-for="capital in question.fourCapitals">
               <button
                  class="btn btn-primary btn-block btn-ans"
                  type="button"
                  name="button"
                  :disabled = "cantClick"
                  @click="checkAnswer(capital,$event)">{{capital}}</button>
            </div>
         </div>
      </div>
      <div class="col-sm-8 offset-sm-2 progressCont"
         v-if="!ended">
         <div class="row">
            <div class="col-sm-12">
               <h4 class="questionNum">Question {{currentQuestion}} of {{maxQuestions}}</h4>
            </div>
         </div>
         <div class="row progressRow">
            <div class="col-sm-12">
               <div class="progress">
                  <div class="progress-bar progress-bar-striped bg-success progressTxt" role="progressbar" v-bind:style="`width: ${((rightAnswers)/maxQuestions)*100}%;`">right: {{rightAnswers}}</div>
               </div>
            </div>
         </div>
         <div class="row">
            <div class="col-sm-12">
               <div class="progress">
                  <div class="progress-bar progress-bar-striped bg-danger progressTxt" role="progressbar" v-bind:style="`width: ${((wrongAnswers)/maxQuestions)*100}%;`">wrong: {{wrongAnswers}}</div>
               </div>
            </div>
         </div>
         <br>
      </div>
   </div>
</template>

<script>
export default {
   data() {
      return {
         questionGen: 'What is the capital of ',
         isTrue: false,
         isClicked: false,
         clicked: "",
         cantClick: false
      }
   },
   props: [
      'question',
      'currentQuestion',
      'maxQuestions',
      'rightAnswers',
      'wrongAnswers',
      "ended"
   ],
   methods: {
      calculatePercentage(number, max) {
         return max / num * 100;
      },
      checkAnswer(capital, btnClicked) {
         this.cantClick = true;
         var self = this;
         self.clickedCity = capital;
         this.isClicked = true;
         var ans = false;
         btnClicked = btnClicked.target;
         btnClicked.style.transition = "background-color .3s";
         btnClicked.style.opacity = "1";
         var len = this.question.fourCapitals.length;
         if (capital === this.question.capital) {
            this.isTrue = true;
            btnClicked.classList.add("btn-success");
         } else {
            this.isTrue = false;
            btnClicked.classList.add("btn-danger");
         }
         setTimeout(function() {
            self.isClicked = false;
            btnClicked.classList.remove("btn-success");
            btnClicked.classList.remove("btn-danger");
            btnClicked.style.transition = "";
            let obj = {
               ansBool: self.isTrue,
               ansClickedCity: self.clickedCity
            }
            self.$emit('clickAnswer', obj);
            setTimeout(function() {
               self.cantClick = false
            }, 400)
         }, 600)
      }
   }
}
</script>

<style lang="css">
.col-pad {
   padding: 15px 20px 10px 20px;
}

.progressCont, .questionCont {
   margin-top:20px;
   margin-bottom: 20px;
}

.progressCont {
   box-shadow: 0px 0px 2px white;
   border-radius: 5px;
}

.questionHead {
   margin-bottom: 20px;
}

.progressTxt {
   color: black;
}

.progressRow {
   margin-bottom: 10px;
}

.btn-ans {
   box-shadow: 0px 0px 2px white;
   transition: box-shadow .1s;
}

.btn-ans:hover {
   box-shadow: 0px 0px 20px white;
}

.questionNum {
   padding: 10px;
}

</style>
