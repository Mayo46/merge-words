<template>
  <div class="custom-container">
    <div class="mt-3">
      <div class="banner-section">
        <div class="right-section">
          <div  class="mb-4 display-block" >
            <h1 class="heading">WordsMerge.om</h1>
          </div>
          <div  class="mb-4 display-block " >
            <div  class="mb-2 display-block" >
              <p class="description">Merge and combine words fast and easily with WordsMerge. Use it for merging words, naming, domaining, Google Ads, PPC, business & brand name generator, etc. Type your keywords in the boxes and press <a class="sub-description link" tabindex="0" @click="mergeText">Merge!</a>
              Or load some sample data 
              (<a class="sub-description link" @click="Domain" tabindex="0">Domaining</a>,
              <a class="sub-description link" @click="link" tabindex="0">Linkbuilding</a>,
              <a class="sub-description link" @click="adwords" tabindex="0">Google Ads</a>)</p>
            </div>
          </div>
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
            label="Enter some words (optional)"
            outlined
            rows="4"
            row-height="50"
            v-model="firstText"
            height="210"
          ></v-textarea>
        </v-col>
        <v-col
          cols="12"
          sm="4"
        >
          <v-textarea
            label="Enter some words (optional)"
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
            label="Enter some words (optional)"
            outlined
            rows="4"
            row-height="50"
            v-model="thirdText"
            height="210"
          ></v-textarea>
        </v-col>
      </v-row>
    </v-container>
    <div class="featured-section">
      <p class="option-tag" @click="showOption=! showOption" > + Extra Options</p>
      <v-btn class="ml-5" color="primary" rounded @click="reset">Reset Field</v-btn>
    </div>
    <div v-show="showOption===true">
      <div class="featured-section mt-3">
        <div class="featured-section-left">
          <span class="featured-title">Separator:</span>
          <v-btn class="mb-2" @click="separate = ''; showCustomField= false;isActive = 'Nothing'" color="#dfe3e9" depressed :class="[  isActive==='Nothing' ? 'active' : '']">Nothing</v-btn>
          <v-btn class="mb-2" @click="separate = ' '; showCustomField= false;isActive = 'Space'" color="#dfe3e9" depressed :class="[  isActive==='Space' ? 'active' : '']">Space</v-btn>
          <v-btn class="mb-2" @click="separate = '-'; showCustomField= false;isActive = 'Minus'" color="#dfe3e9" depressed :class="[  isActive==='Minus' ? 'active' : '']">Minus</v-btn>
          <v-btn class="mb-2" @click="separate = '+'; showCustomField= false;isActive = 'Plus'" color="#dfe3e9" depressed :class="[  isActive==='Plus' ? 'active' : '']">Plus</v-btn>
          <v-btn class="mb-2" @click="showCustomField=true; separate='';isActive = 'Custom'" color="#dfe3e9"  depressed :class="[  isActive==='Custom' ? 'active' : '']">Custom   <v-text-field class="custom-text-field" v-if="showCustomField" v-model="separate" ></v-text-field></v-btn>
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
      <v-container fluid>
        <v-row align="center">
          <v-col class="d-flex" cols="12" sm="6" md="2">
            <span class="featured-title mb-6 pl-0">Add a list of:</span>
          </v-col>
          <v-col class="d-flex" cols="12" sm="6" md="4">
            <v-select :items="items" label="Select Prefixes, Suffixes etc" outlined v-model="selectOption"></v-select>
          </v-col>
          <v-col class="d-flex" cols="12" sm="6" md="4">
            <v-select :items="fields" label="Select Fields" outlined v-model="selectField" v-show="selectOption"></v-select>
          </v-col>
          <v-col class="d-flex" cols="12" sm="6" md="2">
            <v-btn class="mb-8" color="primary" @click="add" v-show="selectOption && selectField" rounded>Add</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </div>
    <h2 class="text-center mt-5 combination-text" v-if="combine.length">{{combine.length}} combinations possible</h2>
    <h2 class="text-center mt-5 combination-text" v-else>0 combinations possible</h2>
    <div class="text-center">
      <v-btn class="mt-5 mb-5" color="primary" @click="mergeText">Merge</v-btn>
    </div>
    <v-textarea outlined rows="4" row-height="50" id="myInput"  v-model="combineText" height="250"></v-textarea>
    <div class="featured-section">
      <v-btn color="primary" @click="copy(false)">Copy All</v-btn>
      <v-btn color="primary" class="ml-5" @click="copy(true)">Copy $ Open GoDaddy</v-btn>
    </div>
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
      activeWrap:'Nothing',
      firstTextArr:[],
      secondTextArr:[],
      thirdTextArr:[],
      selectOption:'',
      selectField:'',
      copyText:'',
      items: [
        {text:'All English Prefixes (re-, dis-, un-, mis- ...)', value:'prefix'},
        {text:'All English Suffixes ( -able, -ness, -ify...)', value:'suffix'},
        {text:'Most Common Domain Prefixes', value:'domainPrefix'},
        {text:'Most Common Domain Suffixes', value:'domainSuffix'},
        {text:'Add English Letters A,B,C,....Z', value:'letters'},
      ],

      fields:[
        {text:'First Text Field', value:'firstText'},
        {text:'Second Text Field', value:'secondText'},
        {text:'Third Text Field', value:'thirdText'},
      ],

      suffix:['able','acity','ade','age','aholic','al','algia','an','ance','ant','ar','ard','arian','arium','ary','ate','ation','ative','cide','cracy',
        'crat','cule','Cy','cycle','dom','dox','ectomy','ed','ee','eer','emia','en','ence','ency','ent','er','ern','escence','ese','esque','ess','est',
        'etic','ette','ful','fy','gam','gamy','gon','gonic','hood','ial','ian','iasis','iatric', 'ible','ic','ile','ily','ine','ing', 'ion','ious','ish',
        'ism','ist','ite','itis','ity','ive','ization','ize','less','let','like','ling','loger', 'logist','log','ly','ment','ness','oid','ology','oma',
        'onym','opia','opsy','or','ory','osis','ous','path','pathy','phile','phobi','phone','phyte','plegia','plegic','pnea','scopy','scope','scribe',
        'script','sect','ship','sion','some','sophy','sophic','th','tion','tome','trophy','tude','ty','ular','uous','ure','ward','ware','wise','y'],

      prefix:['a','an','ante','anti','auto','circum','co','com','con','contra','contro','de','dis','en','ex','extra','hetero','homo',
        'homeo','hyper','il','im','in','ir','inter','intra','intro','macro','micro','mono','non','omni','post','pre','pro','sub','sym',
        'syn','tele','trans','tri','un','uni','up',],

      domainPrefix:["my","the","web","go","super","free","green","net","new","pro","get","mobile","best","all","smart","social","easy","ad","digital",
        "big","live","cloud","top","shop","tech","your","auto","global","love","eco","in","real","mr","art","just","team","one","open","you",
        "world","hot","online","daily","ez","buy","blue","re","blog","click","china","photo","home","dr","travel","it","cyber","game","app",
        "virtual","cool","local","video","red","city","power","info","search","club","quick","we","media","mega","star","design","planet","fast",
        "bio","geo","car","on","life","euro","us","max","good","gold","great","la","think","sky","sun","biz","data","music","business","dream",
        "www","first","project","fun","simple","play","happy","job","dot","book","1","air","co","pc","tv","baby","seo","hi","be","health","sex",
        "no","win","wiki","micro","urban","creative","site","domain","link","news","money","pure","inter","code","me","find","sport","our","sports",
        "ultra","black","fashion","solar","host","brand","deal","so","usa","soft","neo","golf","med","magic","trade","movie","mini","market","fresh",
        "cash","to","hotel","do","internet","simply","got","mac","de","total","poker","thai","master","mi","american","digi","im","face","studio","uni",
        "pet","fly","perfect","group","doctor","cn","pop","zen","ok","le","work","food","share","mo","silver","dj","google","ny","radio","tele","time",
        "alpha","college","future","com","vip","family","phone","active","safe","mc","next","at","wine","mobi","idea","nano","porn","edu","bit","meta",
        "direct","image","space","marketing","audio","key","beauty","fit","flash","un","any","mail","style","metro","stock","up","earth","king","text",
        "golden","eye","for","energy","rock","light","crazy","house","hyper","uk","multi","speed","bid","sound","talk","wow","visual","mad","id","apple",
        "school","gift","asia","fire","clean","instant","office","kid","legal","better","casino","pay","hello","custom","cafe","gay","mind","ms","ip",
        "sexy","party","prime","india","doc","by","map","law","sea","az","buzz","price","post","ace","cheap","pixel","film","clear","sweet","print","geek",
        "nu","full","word","ca","dog","modern","computer","aqua","sales","healthy","ar","ultimate","water","dc","abc","yes","name","forex","am","rapid",
        "yo","only","monster","ai","brain","street","pocket","trip","event","tiny","network","en","ma","nice","foto","wp","techno","man","more","call",
        "al","see","bb","wild","snap","little","coupon","email","zip","show","jet","box","london","ex","omni","cell","ask","now","epic","orange","secure",
        "bank","ever","value","core","bet","organic","royal","color","credit","tap","start","chat","fx","md","iphone","uber","adult","medical","help",
        "career","ur","surf","ilove","soul","lucky","wise","extreme","kids","pink","body","cam","iam","lead","qr","cc","add","service","tweet","joy","is",
        "diamond","tri","newyork","texas","insurance","turbo","discount","high","goto","tag","sa","people","da","bright","rent","elite","run","realestate",
        "bar","chicago","hd","fan","coffee","vision","action","xxx","medi","bi","pin","wedding","card","cat","quality","touch","logo","asian","el","about",
        "pr","games","yoga","traffic","expert","vi","store","clever","campus","nyc","property","crowd","rich","sms","av","living","flex","right","website",
        "natural","bad","fitness","promo","date","express","ed","ice","pre","white","friend","private","day","plus","dev","teen","shopping","angel","pa",
        "spot","sale","ab","voice","bee","nova","iq","se","test","fish","hk","florida","fine","loan","page","fox","android","watch","ticket","like","wi",
        "view","secret","sc","hr","capital","international","make"],


      domainSuffix:["online","web","media","world","net","group","blog","shop","book","store","inc","tech","design","box","now","site","news","app","club","pro",
        "network","plus","tv","zone","link","co","cloud","it","life","works","city","hub","man","list","direct","info","mail","central","marketing","usa",
        "lab","house","solutions","me","art","music","host","home","market","guide","magazine","soft","center","games","land","live","software","space",
        "time","studio","one","talk","search","master","tube","point","apps","star","on","us","mobile","ware","com","cafe","spot","books","planet","mart",
        "line","power","in","place","video","card","forum","company","data","ville","source","page","today","mall","bank","team","press","pad","services",
        "view","corp","deals","biz","game","hq","express","work","service","max","bot","factory","mag","help","base","way","labs","board","nation","portal",
        "ly","systems","share","king","business","bar","map","cast","post","consulting","wire","jobs","project","free","travel","tools","vision","india",
        "watch","up","go","magic","code","bay","logic","trade","connect","digital","guru","finder","report","exchange","china","love","chat","smart","system",
        "social","directory","designs","tree","room","links","er","quest","port","show","mate","international","deal","wise","park","buzz","town","photo",
        "buy","ology","style","global","click","ing","kit","ads","uk","review","tool","hosting","products","gear","cash","expert","management","radio",
        "health","sales","server","sports","america","fun","law","school","people","stream","technology","money","stuff","support","care","wave","ad","day",
        "id","pages","log","pal","machine","station","pay","craft","stock","daily","girl","gallery","fire","track","find","mania","play","light","fx","dog",
        "cam","wiki","pedia","sale","ed","party","channel","tracker","print","depot","asia","job","is","solution","test","key","force","match","tips","buddy",
        "car","sites","insurance","domain","fly","gold","md","street","green","farm","reviews","mark","db","core","os","pack","academy","pod","phone","feed",
        "monkey","capital","mix","networks","plan","you","cards","training","agency","bee","events","wall","fox","guy","manager","ex","camp","creative","monster",
        "boy","top","gate","check","website","connection","masters","videos","engine","trader","builder","la","cat","file","first","pr","all","sky","cube","productions",
        "es","baby","garden","movie","tag","auto","doc","tour","partners","doctor","office","llc","pros","maker","ny","rock","wear","index","hotel","kids","pool","sex",
        "energy","university","geek","studios","sport","cn","fit","family","agent","times","brand","fix","fish","research","fashion","pc","wizard","domains","easy",
        "lounge","fast","local","journal","poker","control","mind","canada","iq","flow","tec","blogs","technologies","photos","con","coach","story","nyc","face","seo",
        "bid","community","safe","tek","rx","well","food","records","cart","ideas","hunter","porn","films","fund","pop","advertising","golf","film","bag","girls",
        "interactive","drive","alert","london","advisor","stop","notes","sense","fitness","ink","age","square","at","supply","images","universe","shopping","photography",
        "trading","band","vault","med","to","air","image","pass","fusion","call","pictures","foryou","friend","centre","wiz","edge","fan","friends","graphics","circle",
        "trip","idea","stars","desk","mob","partner","dot","finance","maps","bus","io","guard","blue","fest","aid","pics","arts","outlet","access","experts","eye","homes",
        "brain","science","realty","sys","scape","zilla","hero","dev","best","date","path","auction","corner","pix","security","note","dream","by","togo","head","ss",
        "internet","pub","text","bridge","price","ventures","jet","scout","men","ok","plaza","sa","voice","gifts","lite","email","trust","focus","zoo","score","movies",
        "zen","cars","forums","case","vip","choice","ball","more","grid","event"],

      letters:['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z']
    }
  },
  watch:{
    firstText(){
      this.firstTextArr= this.firstText.split("\n")
    },
    secondText(){
      this.secondTextArr= this.secondText.split("\n")
    },
    thirdText(){
      this.thirdTextArr= this.thirdText.split("\n")
    },
  },
  computed:{
    combine(){
      let combos = [] //or combos = new Array(2);
      //when all inputs have some data
      if(this.firstText.length>0 && this.secondTextArr.length>0 && this.thirdTextArr.length>0){
        for(let i = 0; i < this.firstTextArr.length; i++)
        {
          for(let j = 0; j < this.secondTextArr.length; j++)
          {
            for(let k = 0; k < this.thirdTextArr.length; k++)
            {
              combos.push(this.firstTextArr[i] + ',' + this.secondTextArr[j] + ',' + this.thirdTextArr[k])
            }
          }
        }
      }
      // when first and second input fields have some data
      else if(this.firstText.length>0 && this.secondTextArr.length>0){
        for(let i = 0; i < this.firstTextArr.length; i++)
        {
          for(let j = 0; j < this.secondTextArr.length; j++)
          {
            //you would access the element of the array as array1[i] and array2[j]
            //create and array with as many elements as the number of arrays you are to combine
            //add them in
            //you could have as many dimensions as you need
            combos.push(this.firstTextArr[i] + ',' + this.secondTextArr[j])
          }
        }
      }
      // when first and third input fields have some data
      else if(this.firstText.length>0 && this.thirdTextArr.length>0){
        for(let i = 0; i < this.firstTextArr.length; i++)
        {
          for(let k = 0; k < this.thirdTextArr.length; k++)
          {
            combos.push(this.firstTextArr[i] + ',' + this.thirdTextArr[k])
          }
        }
      }

      // when second and third input fields have some data
      else if(this.secondText.length>0 && this.thirdTextArr.length>0){
        for(let i = 0; i < this.secondTextArr.length; i++)
        {
          for(let k = 0; k < this.thirdTextArr.length; k++)
          {
            combos.push(this.secondTextArr[i] + ',' + this.thirdTextArr[k])
          }
        }
      }
      // when first  input fields have some data
      else if(this.firstText.length>0){
        for(let i = 0; i < this.firstTextArr.length; i++)
        {
          combos.push(this.firstTextArr[i])
        }
      }
      // when second  input fields have some data
      else if(this.secondText.length>0){
        for(let i = 0; i < this.secondTextArr.length; i++)
        {
          combos.push(this.secondTextArr[i])
        }
      }
      // when third  input fields have some data
      else if(this.thirdText.length>0){
        for(let i = 0; i < this.thirdTextArr.length; i++)
        {
          combos.push(this.thirdTextArr[i])
        }
      }
      return combos;
    }
  },
   beforeMount(){
    this.triggerConversion();
 },
  methods:{
    triggerConversion(){
      setTimeout(function(){
        gtag('config', 'AW-1006620676');
        gtag('event', 'conversion', {'send_to': 'AW-1006620676/2s0cCPLN-90CEISg_98D'});
   }, 60000);
  },
    mergeText(){
      let arr = []
      this.combine.forEach(data => {
        let newArray = data.split(",");
        if(newArray.length > 2){
          arr.push((this.separate === '+' ? this.separate : '') + this.wrapStart + newArray[0] +
            this.separate + newArray[1] + this.separate + newArray[2] + this.wrapEnd)
        }
        else if(newArray.length > 1){
          arr.push((this.separate === '+' ? this.separate : '') + this.wrapStart + newArray[0]
            + this.separate + newArray[1] + this.wrapEnd)
        }
        else {
          arr.push((this.separate === '+' ? this.separate : '') + this.wrapStart + newArray[0] + this.wrapEnd)
        }
      })
      this.combineText = arr.toString().replace(/,/g, '\n')
    },
    wrapping(){
      this.wrapStart='"'
      this.wrapEnd='"'
    },
    Domain(){
      this.firstText= " iphone\n" + "ipad\n" + "ipod\n" + "imac\n" + "macbook"
      this.secondText="world\n" + "life\n" + "web\n" + "planet\n" + "hub\n" + "center\n" + "club\n" + "central\n" + "spot\n" + "base\n" + "stuff"
      this.thirdText=".com\n" + ".net\n" + ".org";
      setTimeout(() => this.mergeText(), 1000)
    
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
    },
    add(){
      let options = [] , field = '' , newArray = []
      switch (this.selectOption)
      {
        case "prefix":
          options = this.prefix
          break;
        case "suffix":
          options = this.suffix
          break;
        case "domainPrefix":
          options = this.domainPrefix
          break;
        case "domainSuffix":
          options = this.domainSuffix
          break;
        case "letters":
          options = this.letters
          break;
      }

      switch (this.selectField)
      {
        case "firstText":
          field = this.firstTextArr
          break;
        case "secondText":
          field = this.secondTextArr
          break;
        case "thirdText":
          field = this.thirdTextArr
          break;
      }
      if(field.length) {
        options.map(option => {
          field.map(text => {
            if(this.selectOption === 'suffix' || this.selectOption === 'domainSuffix'){
              newArray.push(text + option)
            } else {
              newArray.push(option + text)
            }
          })
        })
        switch (this.selectField)
        {
          case "firstText":
            this.firstText = newArray.toString().replace(/,/g, '\n')
            break;
          case "secondText":
            this.secondText = newArray.toString().replace(/,/g, '\n')
            break;
          case "thirdText":
            this.thirdText = newArray.toString().replace(/,/g, '\n')
            break;
        }
      } else {
        switch (this.selectField)
        {
          case "firstText":
            this.firstText = options.toString().replace(/,/g, '\n')
            break;
          case "secondText":
            this.secondText = options.toString().replace(/,/g, '\n')
            break;
          case "thirdText":
            this.thirdText = options.toString().replace(/,/g, '\n')
            break;
        }
      }

    },
    reset(){
      this.firstText=''
      this.secondText=''
      this.thirdText=''
      this.combineText=''

    },
    copy(key){
      let copyText = document.getElementById("myInput");
      copyText.select();
      copyText.setSelectionRange(0, 99999); /* For mobile devices */
      document.execCommand("copy");
      // this.combineText = ''
      if(key === true)
        window.open('https://www.godaddy.com/domains/bulk-domain-search')
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
  color: black;
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
.v-application .primary {
	background-color: #204ecf !important;
	border-color: #204ecf !important;
}
</style>
