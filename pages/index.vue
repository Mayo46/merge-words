<template>
    <div class="custom-container">
      <div class="mt-3">
        <div class="banner-section">
<!--          <div class="left-section">-->
<!--            <img-->
<!--              class="VuetifyLogo"-->
<!--              alt="Vuetify Logo"-->
<!--              src="/vuetify-logo.svg"-->
<!--            >-->
<!--          </div>-->

          <div class="right-section">
            <div  class="mb-4 display-block" >
              <h1 class="heading">mergewords</h1>
            </div>
            <div  class="mb-4 display-block " >
              <div  class="mb-2 display-block" >
                <p class="description">Merge words, fast and easy.</p>
              </div>
              <div  class="mb-2 display-block" >
                <p class="description">Use it for domain registrations,</p>
              </div>
              <div  class="mb-2 display-block"  >
                <p class="description">Google Adwords, whatever.</p>
              </div>
            </div>
            <div class="mb-2 display-block" >
              <p class="sub-description">Type your keywords in the 3 boxes and press
                <a class="sub-description link" tabindex="0" @click="mergeText">Merge!</a>
              </p>
            </div>
            <p class="sub-description">Or load some sample data
              (<a class="sub-description link" @click="Domain" tabindex="0">Domaining</a>,
              <a class="sub-description link" @click="link" tabindex="0">Linkbuilding</a>,
              <a class="sub-description link" @click="adwords" tabindex="0">Adwords</a>)
            </p>
          </div>

        </div>
      </div>
      <v-container fluid>
        <v-row>
          <v-col
            cols="12"
            sm="4"
          >
            <v-textarea
              label="Enter some words here"
              auto-grow
              outlined
              rows="4"
              row-height="50"
              v-model="firstText"
              clearable
            ></v-textarea>
          </v-col>
          <v-col
            cols="12"
            sm="4"
          >
            <v-textarea
              label="Enter some words here"
              height="210"
              outlined
              rows="4"
              row-height="50"
              v-model="secondText"
            ></v-textarea>
          </v-col>
          <v-col
            cols="12"
            sm="4"
          >
            <v-textarea
              label="Enter some words here"
              auto-grow
              outlined
              rows="4"
              row-height="50"
              v-model="thirdText"
              clearable
            ></v-textarea>
          </v-col>


        </v-row>
      </v-container>
      <p class="option-tag" @click="showOption=! showOption" > + Extra Options</p>
      <div v-show="showOption===true">
        <div class="featured-section mt-3">
          <div class="featured-section-left">
              <span class="featured-title">Separator:</span>
              <v-btn @click="separate = ''; showCustomField= false;isActive = 'Nothing'" color="#dfe3e9" depressed :class="[  isActive==='Nothing' ? 'active' : '']">Nothing</v-btn>
              <v-btn @click="separate = ' '; showCustomField= false;isActive = 'Space'" color="#dfe3e9" depressed :class="[  isActive==='Space' ? 'active' : '']">Space</v-btn>
              <v-btn @click="separate = '-'; showCustomField= false;isActive = 'Minus'" color="#dfe3e9" depressed :class="[  isActive==='Minus' ? 'active' : '']">Minus</v-btn>
              <v-btn @click="separate = '+'; showCustomField= false;isActive = 'Plus'" color="#dfe3e9" depressed :class="[  isActive==='Plus' ? 'active' : '']">Plus</v-btn>
              <v-btn @click="showCustomField=true; separate='';isActive = 'Custom'" color="#dfe3e9"  depressed :class="[  isActive==='Custom' ? 'active' : '']">Custom   <v-text-field class="custom-text-field" v-if="showCustomField" v-model="separate" ></v-text-field></v-btn>
          </div>
          <div class="">
            <div class="">
              <span class="featured-title">Wrap in:</span>
              <v-btn @click="wrapStart=''; wrapEnd='';activeWrap = 'Nothing'" color="#dfe3e9" depressed :class="[  activeWrap==='Nothing' ? 'active' : '']">Nothing</v-btn>
              <v-btn @click="wrapping(); activeWrap=''"  color="#dfe3e9" depressed :class="[  activeWrap==='' ? 'active' : '']">" "</v-btn>
              <v-btn @click="wrapStart='['; wrapEnd=']';activeWrap = '[]'" color="#dfe3e9" depressed :class="[  activeWrap==='[]' ? 'active' : '']">[]</v-btn>
            </div>
          </div>
        </div>
      </div>
      <h2 class="text-center mt-5 combination-text" v-if="combineText">1 combinations possible</h2>
      <h2 class="text-center mt-5 combination-text" v-else>0 combinations possible</h2>
      <div class="text-center">
        <v-btn class="mt-5 mb-5" color="primary" @click="mergeText">Merge</v-btn>
      </div>
      <v-textarea
        auto-grow
        outlined
        rows="4"
        row-height="50"
        v-model="combineText"
        clearable
      ></v-textarea>
    </div>
</template>
<script>
export default {
  data(){
   return{
     showOption:false,
     showCustomField:false,
     firstText:"",
     secondText:"",
     thirdText:"",
     combineText:"",
     separate: ' ',
     wrapStart: '',
     wrapEnd: '',
     isActive:'Space',
     activeWrap:'Nothing'
   }
  },
  methods:{
   mergeText(){
     console.log(this.firstText)
     console.log(this.secondText)
     console.log(this.thirdText)
     if(this.firstText || this.secondText || this.thirdText){
       this.combineText= this.wrapStart + this.firstText + this.separate + this.secondText + this.separate +this.thirdText + this.wrapEnd
     }
     console.log(this.combineText)

   },
    wrapping(){
     this.wrapStart='"'
      this.wrapEnd='"'
    },
    Domain(){
     this.firstText= " iphone\n" + "ipad\n" + "ipod\n" + "imac\n" + "macbook"
     this.secondText="world\n" + "life\n" + "web\n" + "planet\n" + "hub\n" + "center\n" + "club\n" + "central\n" + "spot\n" + "base\n" + "stuff"
     this.thirdText=".com\n" + ".net\n" + ".org"
   },
    link(){
     this.firstText="mountaineering\n" + "climbing\n" + "hiking\n" + "trekking"
     this.secondText="websites\n" + "links\n" + "\"add url\"\n" + "\"suggest a site\""
     this.thirdText="intitle:list\n" + "inurl:resources\n" + "OR \"suggest URL\"\n" + "OR resources"
    },
    adwords(){
     this.firstText="ladies\n" + "women\n" + "designer\n" + "fashion"
     this.secondText="shoes\n" + "boots\n" + "sandals\n" + "stiletto heels"
     this.thirdText="New York\n" + "New Jersey\n" + "Long Island City\n" + "Manhattan"
    }
  }
}
</script>
<style scoped>
.custom-container{
  max-width: 1000px;
  margin: 0 auto;
}
.banner-section{
  width: calc(100% + 32px);
  margin: -16px;
  align-items: center;
  display: flex;
  flex-wrap: wrap;
  box-sizing: border-box;
  padding: 16px;
}
@media (min-width: 768px){
  .left-section {
    flex-grow: 0;
    max-width: 33.333333%;
    flex-basis: 33.333333%;
  }
}
.left-section{
  margin: 0;
  box-sizing: border-box;
}
@media (min-width: 768px){
  .right-section{
    flex-grow: 0;
    max-width: 66.666667%;
    flex-basis: 66.666667%;
  }
}
.right-section{
  margin: 0;
  box-sizing: border-box;
  padding: 16px;
}

.VuetifyLogo {
  height: 180px;
  width: 180px;
  transform: rotateY(560deg);
  animation: turn 3.5s ease-out forwards 1s;
}

@keyframes turn {
  100% {
    transform: rotateY(0deg);
  }
}
.display-block{
  display: block;
}

.heading{
  color: #455065;
  font-size: 2.25rem;
  font-weight: 300;
  line-height: 1.25em;
  text-shadow: rgb(51, 51, 51) 0px 1px 1px;
}
.description{
  font-size: 1.25rem;
  line-height: 1.5em;
  font-weight: 400;
  color: #455065;
}
.sub-description{
  color: #455065;
  font-size: 1rem;
  line-height: 1.5em;
  font-weight: 400;
}
.link{
  color: #204ecf;
  cursor:pointer;
}



.featured-section{
  display: flex;
  flex-wrap: wrap;
  box-sizing: border-box;
  align-items: flex-start;
  width: calc(100% + 16px);
  margin: -8px;
}
@media (min-width: 992px){
  .featured-section-left {
    flex-grow: 0;
    max-width: 66.666667%;
    flex-basis: 66.666667%;
  }
}
.featured-section-left {
  margin-bottom: 8px;
  box-sizing: border-box;
  display: inline-block;

}
.option-height{
max-height: 200px;
}
.option-tag{
  color: #204ECF;
  padding-left: 16px;
  /*max-height: 0px;*/
  /*overflow: hidden;*/
  /*transition: max-height 0.4s ease-out 0s;*/

}
.option-tag:hover{
  text-decoration: underline;
  cursor: pointer;
}
.combination-text{
  color: #455065;
  font-size: 1.1428571428571428rem;
  font-weight: 400;
  line-height: 1.5;
}


.featured-title{
  display: inline-block;
  margin-right: 1rem;
  min-width: 4.5rem;
  padding-left: 16px;
}
/*.custom-text-field::v-deep .v-input__slot{*/
/*  width: 25%;*/
/*}*/
.custom-text-field::v-deep .v-input__control{
  width: 35px;
}
.active{
  background-color: #B6C0CD !important;
  /*color: blue;*/
}
</style>
