<template>
  <div class="mt-3">
    <div class="banner-section">
      <div class="left-section">
        <img
          class="VuetifyLogo"
          alt="Vuetify Logo"
          src="/vuetify-logo.svg"
        >
      </div>

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
            <a class="sub-description link" tabindex="0">Merge!</a>
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
</template>

<script>
export default {
  name: "Banner.vue",
  methods:{
    Domain(){
      // this.firstText= " iphone\n" + "ipad\n" + "ipod\n" + "imac\n" + "macbook"
      // this.secondText="world\n" + "life\n" + "web\n" + "planet\n" + "hub\n" + "center\n" + "club\n" + "central\n" + "spot\n" + "base\n" + "stuff"
      // this.thirdText=".com\n" + ".net\n" + ".org"
    },
    link(){
      // this.firstText="mountaineering\n" + "climbing\n" + "hiking\n" + "trekking"
      // this.secondText="websites\n" + "links\n" + "\"add url\"\n" + "\"suggest a site\""
      // this.thirdText="intitle:list\n" + "inurl:resources\n" + "OR \"suggest URL\"\n" + "OR resources"
    },
    adwords(){
      // this.firstText="ladies\n" + "women\n" + "designer\n" + "fashion"
      // this.secondText="shoes\n" + "boots\n" + "sandals\n" + "stiletto heels"
      // this.thirdText="New York\n" + "New Jersey\n" + "Long Island City\n" + "Manhattan"
    }
  }
}
</script>

<style scoped>

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
</style>

