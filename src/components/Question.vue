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
                  class="btn btn-primary btn-block"
                  type="button"
                  name="button"
                  @click="checkAnswer(capital,$event)">{{capital}}</button>
            </div>
         </div>
      </div>
      <div class="col-sm-8 offset-sm-2 progressCont">
         <div class="row"
            v-if="!ended">
            <div class="col-sm-12">
               <h4>Question {{currentQuestion}} of {{maxQuestions}}</h4>
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
   data(){
      return {
         questionGen: 'What is the capital of ',
         isTrue: false,
         isClicked: false,
         clicked :""
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
      checkAnswer(capital,btnClicked){
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
         setTimeout(function(){
            self.isClicked = false;
            btnClicked.classList.remove("btn-success");
            btnClicked.classList.remove("btn-danger");
            btnClicked.style.transition = "";
            let obj = {
               ansBool : self.isTrue,
               ansClickedCity : self.clickedCity
            }
            self.$emit('clickAnswer', obj); //
         },600)
      }
   }
}
</script>

<style lang="css">
.col-pad {
   padding: 10px;
   padding-left: 20px;
   padding-right: 20px;
}

.progressCont, .questionCont {
   margin-top:20px;
   margin-bottom: 20px;
}

.questionHead {
   margin-bottom: 20px;
}

.progressTxt {
   color: black;
}

.progressRow {
   margin-bottom: 10px;
   margin-top: 10px;
}

</style>
