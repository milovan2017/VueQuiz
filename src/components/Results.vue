<template lang="html">
   <div class="row resCont"
      v-if="ended">
      <div class="col-sm-12">
         <h2 class="titleRes">Results</h2>
      </div>
      <div class="col-sm-8 offset-sm-2 table_cont">
         <table class="table table-hover">
            <thead>
               <tr>
                  <th>Num #</th>
                  <th>Country</th>
                  <th>Your Answer</th>
                  <th>Right Answer</th>
               </tr>
            </thead>
            <tbody>
               <tr v-for="(ans,index) in myAnswers.answerArr"
                  v-bind:class="{'table-success': ans.isAnswerTrue, 'table-danger': !ans.isAnswerTrue}"
                  >
                  <td>{{index+1}}</td>
                  <td> {{ans.country}}</td>
                  <td> {{ans.answer}}</td>
                  <td
                     class="success"
                     v-if="!show">?</td>
                  <td
                     v-if="show"
                     > {{ans.capital}}</td>
               </tr>
            </tbody>
         </table>
      </div>
      <div class="col-sm-12 btnRes">
         <button
            type="button"
            name="button"
            class="btn btn-primary start-btn"
            @click="startAgain"
            >Start Again</button>
            <button
               class="btn btn-success headReveal"
               @click="show = true"
               >Click To Reveal Answers</button>
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
      startAgain() {
         this.$emit('startAgain', 'poruka neka')
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
      max-height: 600px;
      overflow-y: auto;
      box-shadow: 0px 0px 50px white;
      padding-top: 15px;
      border-radius: 5px;
   }

   .table th{
      border-top: none;
   }

    .table td {
       border-top: none;
    }

    .table thead th {
       border-bottom: none;
      color:white;
    }

   .headReveal {
      cursor: pointer
   }

   .start-btn {
      margin:20px;
   }

   .titleRes {
      margin-bottom: 20px;
   }

   .btnRes {
      margin-top: 10px;
      margin-bottom: 10px;
   }
</style>
