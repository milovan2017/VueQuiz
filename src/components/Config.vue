<template lang="html">
   <div class="row cnf">
      <div class="col-sm-12">
         <h2 class="title">Choose Continents</h2>
      </div>
      <div class="row mx-auto">
         <div class="col-sm-4 contPad" v-for="(continent, index) in config.continentOption">
            <div class="">
               <img
                  :src="continent.imgUrl"
                  alt="continent.name"
                  class="contImg"
                  :class="{chosen:continent.chosen, notchosen:!continent.chosen}"
                  @click="clickCont(index)">
            </div>
            <div class="">
               <label>{{ continent.name }}</label>
            </div>
         </div>
         <div class="col-sm-12">
            <button
               type="button"
               name="button"
               class="btn btn-primary start-btn"
               v-bind:class="{'btn-success': isOneChosen}"
               :disabled = "!isOneChosen"
               @click="start">Start Quiz</button>
         </div>
      </div>
   </div>
</template>

<script>
export default {
   data() {
      return {
         citiesArr: [],
         chosenObj: [],
         isOneChosen: false,
         config: {
            continentOption: [{
                  name: 'World',
                  imgUrl: 'src/assets/cont_imgs/world.jpg',
                  chosen: false
               },
               {
                  name: 'Europe',
                  imgUrl: 'src/assets/cont_imgs/europe.jpg',
                  chosen: false
               },
               {
                  name: 'Africa',
                  imgUrl: 'src/assets/cont_imgs/africa.jpg',
                  chosen: false
               },
               {
                  name: 'North America',
                  imgUrl: 'src/assets/cont_imgs/northA.jpg',
                  chosen: false
               },
               {
                  name: 'South America',
                  imgUrl: 'src/assets/cont_imgs/southA.jpg',
                  chosen: false
               },
               {
                  name: 'Asia',
                  imgUrl: 'src/assets/cont_imgs/asia.jpg',
                  chosen: false
               }
            ],
            continentAll: false
         }
      }
   },
   methods: {
      start() {
         this.chosenObj = [];
         var length = this.citiesArr.length;
         for (var i = 0; i < length; i++) {
            for (var j = 0; j < this.chosenArr.length; j++) {
               if (this.chosenArr[j] == this.citiesArr[i].ContinentName) {
                  let country = this.citiesArr[i].CountryName,
                     capital = this.citiesArr[i].CapitalName;
                  let obj = {
                     country,
                     capital
                  };
                  this.chosenObj.push(obj);
               }
            }
         }
         let questionArr = this.chosenObj;
         this.$emit('getQuestionsArr', questionArr);
      },
      clickCont(index) {
         let clickedCont = this.config.continentOption[index];
         let contArr = this.config.continentOption;
         let contArrLen = contArr.length;
         clickedCont.chosen = !clickedCont.chosen;
         if (clickedCont.name === "World") {
            if (clickedCont.chosen) {
               for (var i = 1; i < contArrLen; i++) {
                  contArr[i].chosen = true;
               }
            } else {
               for (var i = 1; i < contArrLen; i++) {
                  contArr[i].chosen = false;
               }
            }
         }
         this.chosenArr = [];
         for (var i = 0; i < this.config.continentOption.length; i++) {
            if (this.config.continentOption[i].chosen == true) {
               this.chosenArr.push(this.config.continentOption[i].name);
            }
         }
         if (this.chosenArr.length > 0) {
            this.isOneChosen = true;
         } else {
            this.isOneChosen = false;
         }
      }
   },
   created() {
      let url = '/VueQuiz/src/assets/country-capitals.json'; //paziti pri dev /country-capitals.json deploy : /VueQuiz/src/assets/country-capitals.json
      this.$http.get(url).then(response => {
         this.citiesArr = response.body;
      }, response => {
         // error callback
      });
   }
}
</script>

<style lang="css" scoped>

.contImg {
   border-radius: 50%;
   transition: border .3s;
   box-shadow: 0px 0px 10px lightgreen;
   cursor: pointer;
}

.chosen {
   border: 8px solid green;
}

.notchosen {
   border: 8px solid #00a8ff;
}

.title {
   margin: 20px;
}

.start-btn {
   padding: 10px 100px;
   font-size: 20px;
   opacity: 1;
   border: 2px solid white;
   margin: 15px;
}

.contPad {
   padding-top: 20px;
}

</style>
