<template lang="html">
   <div class="row"
      v-if="ended">
      <div class="col-sm-8 offset-sm-2 table_cont">
         <table class="table table-hover">
            <thead>
               <tr>
                  <th>#</th>
                  <th>Country</th>
                  <th>Your Answer</th>
                  <th
                     @click="show = true"
                     >Click to reveal answers</th>
               </tr>
            </thead>
            <tbody>
               <tr v-for="(ans,index) in myAnswers.answerArr"
                  v-bind:class="{'table-success': ans.isAnswerTrue, 'table-danger': !ans.isAnswerTrue}"
                  >
                  <td>{{index+1}}</td>
                  <td> {{ans.country}}</td>
                  <td> {{ans.answer}}</td>
                  <td v-show="show"> {{ans.capital}}</td>
               </tr>
            </tbody>
         </table>
      </div>
      <div class="col-sm-12">
         <button
            type="button"
            name="button"
            class="btn btn-primary start-btn"
            @click="startAgain"
            >Start Again</button>
      </div>
   </div>
</template>

<script>
export default {
  data() {
    return {
      show: false
    }
  },
  props: [
    'myAnswers',
    "ended"
  ],
  methods: {
     startAgain(){
        console.log('aa');
        //this.ended = false;
        this.$emit('startAgain','poruka neka')
     }
  }
}
</script>

<style lang="css">
   .isTrue {
      background-color: green;
   }
   .notTrue {
      background-color: red;
   }
   .table_cont {
      height: 600px;
      overflow-y: scroll;
      margin-bottom: 50px;
   }
</style>
