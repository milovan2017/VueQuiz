<template lang="html">
   <div class="row"
      v-if="ended">
      <div class="col-sm-12 table_cont">
         <table class="table table-hover">
            <thead>
               <tr class="d-flex">
                  <th>#</th>
                  <th>Country</th>
                  <th>Your Answer</th>
                  <th>Capital</th>
               </tr>
            </thead>
            <tbody>
               <tr v-for="(ans,index) in myAnswers.answerArr"
                  class="d-flex"
                  v-bind:class="{'table-success': ans.isAnswerTrue, 'table-danger': !ans.isAnswerTrue}"
                  @click="show = true" >
                  <td>{{index}}</td>
                  <td> {{ans.country}}</td>
                  <td> {{ans.answer}}</td>
                  <td v-show="show"> {{ans.capital}}</td>
               </tr>
            </tbody>
         </table>
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
