<template>
    <div class="custom-container">
      <div class="mt-3">
        <div class="banner-section">
          <div class="right-section">
            <div  class="mb-4 display-block" >
              <h1 class="heading">WordsMerge.com</h1>
            </div>
            <div  class="mb-4 display-block " >
              <div  class="mb-2 display-block" >
                <p class="description">Merge and combine words fast and easy.</p>
              </div>
              <div  class="mb-2 display-block" >
                <p class="description">Use it for domain registrations, Google Adwords, or whatever. </p>
              </div>
              <div  class="mb-2 display-block"  >
                <p class="description">Or as a business/domain name generator.</p>
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
            <v-col class="d-flex" cols="12" sm="6" md="4">
              <v-select :items="items" label="Select Prefix or Suffix" outlined v-model="selectOption"></v-select>
            </v-col>
            <v-col class="d-flex" cols="12" sm="6" md="4">
              <v-select :items="fields" label="Select Fields" outlined v-model="selectField" v-show="selectOption"></v-select>
            </v-col>
            <v-col class="d-flex" cols="12" sm="6" md="4">
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
      <v-textarea outlined rows="4" row-height="50" id="myInput" v-model="combineText" height="250"></v-textarea>
      <div class="featured-section">
        <v-btn color="primary" @click="copy(false)">Copy All</v-btn>
        <v-btn color="primary" class="ml-5" @click="copy(true)">Open GoDaddy</v-btn>
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
       {text:'All English prefixes', value:'prefix'},
       {text:'All English suffix', value:'suffix'},
       {text:'Most Common domain prefix', value:'domainPrefix'},
       {text:'Most Common domain Suffix', value:'domainSuffix'},
       {text:'Add English letters A,B,C,....Z', value:'letters'},
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

     prefix:['a-','an-','ante-','anti-','auto-','circum-','co-','com-','con-','contra-','contro-','de-','dis-','en-','ex-','extra-','hetero-','homo-',
       'homeo-','hyper-','il-','im-','in-','ir-','inter-','intra-','intro-','macro-','micro-','mono-','non-','omni-','post-','pre-','pro-','sub-','sym-',
       'syn-','tele-','trans-','tri-','un-','uni-','up',],

     domainPrefix:['my','the','web','go','super','free','green','net','new','pro','get','mobile','best','all','smart','social','easy','ad','digital','big',
       'live','cloud','top','shop','tech','your','auto','global','love','eco','in','real','mr','art','just','team','one','open','you','world','hot','online',
       'daily','ez','buy','blue','re','blog','click','china','photo','home','dr','travel','it','cyber','game','app','virtual','cool','local','video','red',
       'city','power','info','search','club','quick','we','media','mega','star','design','planet','fast','bio','geo','car','on','life','euro','us','max',
       'good','gold','great','la','think','sky','sun','biz','data','music','business','dream','www','first','project','fun','simple','play','happy','job',
       'dot','book','1','air','co','pc','tv','baby','seo','hi','be','health','sex','no','win','wiki','micro','urban','creative','site','domain','link','news',
       'money','pure','inter','code','me','find','sport','our','sports','ultra','black','fashion','solar','host','brand','deal','so','usa','soft','neo','golf',
       'med','magic','trade','movie','mini','market','fresh','cash','to','hotel','do','internet','simply','got','mac','de','total','poker','thai','master','mi',
       'american','digi','im','face','studio','uni','pet','fly','perfect','group','doctor','cn','pop','zen','ok','le','work','food','share','mo','silver','dj',
       'google','ny','radio','tele','time','alpha','college','future','com','vip','family','phone','active','safe','mc','next','at','wine','mobi','idea','nano',
       'porn','edu','bit','meta','direct','image','space','marketing','audio','key','beauty','fit','flash','un','any','mail','style','metro','stock','up','earth',
       'king','text','golden','eye','for','energy','rock','light','crazy','house','hyper','uk','multi','speed','bid','sound','talk','wow','visual','mad','id',
       'apple','school','gift','asia','fire','clean','instant','office','kid','legal','better','casino','pay','hello','custom','cafe','gay','mind','ms','ip','sexy',
       'party','prime','india','doc','by','map','law','sea','az','buzz','price','post','ace','cheap','pixel','film','clear','sweet','print','geek','nu','full','word',
       'ca','dog','modern','computer','aqua','sales','healthy','ar','ultimate','water','dc','abc','yes','name','forex','am','rapid','yo','only','monster','ai','brain',
       'street','pocket','trip','event','tiny','network','en','ma','nice','foto','wp','techno','man','more','call','al','see','bb','wild','snap','little','coupon',
       'email','zip','show','jet','box','london','ex','omni','cell','ask','now','epic','orange','secure','bank','ever','value','core','bet','organic','royal','color',
       'credit','tap','start','chat','fx','md','iphone','uber','adult','medical','help','career','ur','surf','ilove','soul','lucky','wise','extreme','kids','pink',
       'body','cam','iam','lead','qr','cc','add','service','tweet','joy','is','diamond','tri','newyork','texas','insurance','turbo','discount','high','goto','tag','sa',
       'people','da','bright','rent','elite','run','realestate','bar','chicago','hd','fan','coffee','vision','action','xxx','medi','bi','pin','wedding','card','cat',
       'quality','touch','logo','asian','el','about','pr','games','yoga','traffic','expert','vi','store','clever','campus','nyc','property','crowd','rich','sms','av',
       'living','flex','right','website','natural','bad','fitness','promo','date','express','ed','ice','pre','white','friend','private','day','plus','dev','teen',
       'shopping','angel','pa','spot','sale','ab','voice','bee','nova','iq','se','test','fish','hk','florida','fine','loan','page','fox','android','watch','ticket',
       'like','wi','view','secret','sc','hr','capital','international','make','oz','universal','swiss','pic','aero','auction','ac','liquid','mister','bike',
       'raw','student','hit','care','ec','cd','save','learn','diy','grand','sf','ocean','paper','small','united','every','db','finance','software',
       'insta','electric','well','as','girl','ap','fb','community','track','file','mm','twitter','list','california','ideal','java','premium','si',
       'zero','tax','tube','personal','ag','astro','ink','li','moon','christian','boston','wonder','point','camp','extra','garden','canada','land',
       'corporate','trans','mob','can','why','bay','zoom','heart','em','sim','globe','japan','picture','via','moto','toy','trend','diet','ready',
       'public','facebook','celebrity','national','rx','cs','tour','cal','tru','ic','soccer','try','classic','hair','ga','source','indian','robo',
       'dragon','plan','snow','aa','affiliate','double','solo','fat','sell','english','oc','awesome','arab','human','sure','agent','worldwide',
       'island','song','hosting','model','poly','naked','holiday','sino','viet','ne','mix','inside','mortgage','wave','article','dollar','cross',
       'study','tiger','sub','chinese','socialmedia','amazon','bc','wireless','lab','sp','dance','scan','tree','funny','luxury','beach','security',
       'phoenix','metal','vintage','stream','look','sh','pi','smile','freedom','austin','fair','nj','review','hollywood','trust','stone','crystal',
       'unique','motor','cy','nature','ba','amazing','di','old','ct','beer','spa','special','park','miami','joe','central','west','yellow','class',
       'deep','miss','company','howto','retail','hip','intelli','financial','story','rate','ss','giga','sm','an','eu','fantasy','dynamic','match',
       'beyond','god','church','js','check','realty','robot','main','farm','es','oh','retro','lady','dental','sonic','premier','dating','nc','select',
       'safety','know','wood','handy','laser','tea','gen','bd','connect','iron','america','led','talent','purple','va','indie','dark','young','night',
       'delta','ipad','iran','beta','fusion','indo','out','budget','random','lux','football','flip','band','official','spring','mass','monkey','cute',
       'senior','swift','ru','spark','rc','over','designer','screen','compu','race','os','st','learning','country','its','mt','gem','coach','seek',
       'ameri','startup','terra','jobs','eagle','cine','shoe','pacific','absolute','success','liberty','carbon','quantum','mark','build','last','na',
       'order','spy','solid','seattle','server','vita','focus','mountain','tx','platinum','spin','venture','sugar','base','flow','charity','tu','io',
       'access','electro','jp','download','anti','latin','indy','bj','ge','ds','road','pm','ski','zoo','au','interactive','fin','siam','basic',
       'say','sign','science','craft','wind','rainbow','icon','guitar','drive','single','choice','ads','hard','dn','profit','rad','mighty','zone',
       'speedy','skin','gogo','ebook','long','target','press','gps','alt','east','dvd','atlanta','inno','ta','complete','wa','ps','river','self',
       'tek','cm','join','math','adventure','php','need','vital','feed','africa','ce','cg','ww','system','ee','jam','echo','sb','vacation','cb',
       'horse','forum','jazz','electronic','tt','bingo','apps','glass','blu','ten','vid','sam','bo','mod','wealth','very','boat','two','mission',
       'cv','sz','dig','hire','and','fix','wholesale','town','pizza','tc','con','spirit','dd','or','bargain','animal','korea','mag','sol','porno',
       'guru','sharp','pt','sunny','cruise','pharma','square','cap','edge','expo','cu','wall','lasvegas','thebest','kc','neuro','gaming','grow','pix',
       'slim','anime','strong','tec','infinite','ro','van','mom','reel','maxi','discover','candy','omega','ri','what','karma','line','viva','colorado',
       'dallas','everything','flower','sg','sy','executive','meet','nb','forever','create','lets','rental','kitchen','hybrid','back','beat','pod',
       'houston','jewelry','jo','denver','ra','peak','hub','fund','gp','southern','sw','bear','invest','bg','thedaily','giant','survey','twit','pick',
       'cinema','desi','act','gadget','channel','panda','buddy','original','education','support','general','portal','camera','restaurant','content','french',
       'read','move','outdoor','head','use','agile','ir','oil','canadian','holo','graphic','fuck','trading','su','log','cr','fab','set','fa','holy','ja',
       'clip','mx','culture','ali','product','motion','everyday','ninja','jump','viral','dna','wap','gym','professional','flight','copy','hand','ie',
       'summer','boom','center','memory','wish','enter','ci','coco','drink','dealer','vote','hawaii','technology','pool','mb','zap','own','gamer','vn',
       'iso','ya','second','nw','pub','macro','ruby','pp','tw','cp','lion','dm','infinity','beautiful','how','ak','bt','arizona','ho','artist','aaa',
       'hope','ka','weather','te','guide','xtreme','dirty','youth','ev','ha','atomic','mu','twin','tool','ii','advanced','north','logic','evo','inner',
       'hey','union','qq','vet','grid','menu','ch','ko','ti','friendly','african','ts','score','send','lo','op','room','contact','wire','et','this',
       'lawyer','quote','gas','lazy','faith','cycle','change','dg','university','her','training','om','storm','tennis','gm','today','ion','penny','tao',
       'kiss','steel','remote','gd','wo','europe','enviro','russian','write','ze','bible','tokyo','enjoy','directory','bill','hp','ring','greek',
       'political','fm','hydro','huge','jc','wellness','ship','circle','arts','gun','gt','paris','seed','mint','kiwi','hero','rv','lv','ohio','military',
       'rose','gg','triple','seven','give','tom','rain','dubai','ea','offer','hy','gi','tm','mk','spanish','jersey','western','flying','genius','eat',
       'ht','valley','gig','reality','cook','jj','gs','dan','pen','san','apex','lost','mn','irish','para','br','pan','todays','war','honey','ns','mojo',
       'weare','tel','demo','theme','will','gulf','alaska','boss','user','aus','gear','mp','arc','peer','commercial','fi','tablet','cosmic','tab','excel',
       'dl','truck','side','case','alex','rs','chocolate','wifi','consumer','toronto','bird','wicked','franchise','xl','cube','jd','ki','hb','sl','vr',
       'lean','nex','village','train','bon','fl','galaxy','dyna','pad','drop','noble','research','push','taxi','note','south','mma','salon','cf','ni',
       'citi','tip','superior','hobby','aim','philly','four','if','smarter','performance','foot','gov','men','sync','fs','sandiego','utah','women','comp',
       'voip','sys','miracle','popular','chef','coin','hs','sos','bubble','nerd','bk','orlando','pe','hack','casa','wired','skill','celeb','opti','fuel',
       'major','lan','pretty','swap','goal','dutch','peace','bridge','wm','mg','chi','lol','banner','port','nude','mall','ray','concept','he','vc','url',
       'cherry','arch','ride','worldof','uc','vietnam','child','bag','sage','fortune','ks','palm','sci','lite','moving','pal','linux','positive','cosmo',
       'place','ib','essential','cold','silent','wee','tattoo','vo','du','rev','bus','friends','bold','lifestyle','proxy','savvy','mye','niche','rank',
       'washington','girls','area','pb','exclusive','nutri','cover','allabout','vista','debt','spider','po','marine','chic','nz','virgin','alien','accu',
       'sk','furniture','gz','erotic','caribbean','ego','ph','resume','divine','after','ae','georgia','thunder','form','final','yu','plant','bell','way',
       'myown','tune','lex','impact','german','sem','ping','ceo','myweb','short','eb','want','ben','goo','knowledge','certified','building','common',
       'advertising','boy','industry','oregon','portland','passion','ep','ghost','shadow','mobil','may','agri','fancy','flat','cake','thebig','healthcare',
       'vina','dp','mama','wiz','binary','jack','nfc','swing','killer','script','stage','board','bug','er','bitcoin','intelligent','task','sat','aloha',
       'trader','tr','pak','sunshine','mas','cpa','funky','int','wolf','bs','state','legacy','byte','desk','gb','supreme','patent','ff','matrix','css',
       'desert','chem','met','step','running','thenew','pk','deals','gene','investment','hockey','apartment','yahoo','zz','fight','stat','gc','able',
       'persian','il','jb','cable','tee','empire','michigan','cars','yacht','take','launch','sleep','client','carolina','lee','condo','mon','summit',
       'gallery','rare','th','txt','virginia','wet','midwest','ky','six','mango','industrial','brooklyn','ty','zombie','christmas','sin','ah','quest',
       'oo','ve','she','famous','with','whole','excellent','root','houseof','detroit','lotto','comic','mono','maine','dns','fishing','rt','glow',
       'crown','teacher','france','winter','rebel','aussie','thisis','artof','hidden','gl','tera','dash','bp','fc','thailand','answer','mv','brazil',
       'thegreen','advance','enterprise','quik','bob','lime','control','cms','into','corp','cuba','weed','mine','atlas','fruit','captain','broker','ku',
       'af','cl','income','jr','ezy','slow','gr','pearl','mj','ratemy','vin','creditcard','sj','exchange','type','hh','choose','sr','vox','native',
       'drug','buffalo','ui','italian','field','five','bing','brown','compare','who','uu','paint','weightloss','baseball','lemon','alfa','partner','lin',
       'min','feel','jm','citizen','gourmet','evil','speak','far','grey','yoo','british','xo','cj','xy','realtime','bikini','ball','vitamin','soho',
       'intra','tex','european','racing','jungle','muscle','indigo','ivy','smooth','droid','index','chip','ke','pets','deco','stop','pilot','inet',
       'icloud','rap','wh','cad','standard','socal','brilliant','solution','block','shanghai','tn','storage','maximum','memo','weekly','bella','tribal',
       'myi','vivid','woo','mar','wide','mba','je','nh','spiritual','xm','kk','luv','lake','tutor','synergy','kool','table','dial','frog','neon',
       'his','laptop','tj','mlm','saudi','mexico','cooking','nordic','acme','paradise','bali','dear','switch','dive','rail','three','fake','some',
       'leader','not','talking','egg','dead','john','crm','hiphop','innovation','jewish','garage','pd','that','reno','shark','truth','information','don',
       'ken','bot','loop','vm','taiwan','investor','glo','omg','rio','km','lotus','current','ia','bu','td','vertical','of','singapore','sec','scuba',
       'vancouver','finda','exotic','jewel','naughty','thought','loco','too','northern','montana','atlantic','construction','han','sand','losangeles','kb',
       'customer','tampa','arcade','signature','tecno','hc','load','profile','eg','nm','kin','option','hu','mike','sail','pg','mrs','kings','victory',
       'sustainable','ls','berry','pv','coastal','pos','commerce','afro','equi','tamil','pirate','iowa','gray','ws','jk','woman','keep','insure','pac',
       'alternative','ant','todo','shared','za','recipe','geta','realtor','jay','australian','desktop','vt','chess','rr','mmo','drum','dk','age',
       'cellphone','prize','brick','latino','swim','ao','herbal','imagine','bamboo','leo','freelance','bonus','battle','lock','piano','splash','gorilla',
       'titan','maui','dv','charlotte','indi','eden','manhattan','turk','low','tim','cq','ebay','intel','insight','arrow','ortho','ua','archi','mystic',
       'nashville','immo','uae','hn','vs','odd','fu','lovely','visa','front','kingdom','slick','catholic','ux','nd','wallstreet','grace','webdesign',
       'nt','mid','broad','wisdom','trendy','bull','pack','bulk','flexi','kick','img','villa','dual','generation','sv','rss','duo','ero','tasty','bond',
       'fo','fantastic','ml','fiber','winning','ei','amber','polar','hunt','innovative','busy','discovery','pass','primary','aj','strategic','factory',
       'equity','spice','david','staff','estate','banana','agency','visit','fe','history','iwant','nv','nutrition','groove','sticky','vegan','nat',
       'webhosting','xx','atom','quiz','rd','comedy','hongkong','gambling','rush','bliss','thin','bed','bh','unlimited','findmy','nation','safari',
       'teach','hq','welove','under','petro','mood','ut','parent','wed','agro','nine','veri','japanese','supply',
       'mommy','lu','large','australia','champion','record','luxe','shine','dia','reading','israel','jesus','ol','lc','heat','part','mundo','down',
       'jordan','korean','deluxe','luna','precision','afri','kindle','gf','ido','per','ax','sup','off','pico','nurse','kit','rm','message','nevada',
       'juicy','lease','merchant','movies','aspen','reach','experience','stereo','comfort','maryland','revolution','vb','soc','sterling','gator','slide',
       'lg','boomer','chrome','musical','destination','novo','wing','fame','window','colour','indiana','sn','puppy','xs','mw','pussy','kosher','guy',
       'herb','affordable','proto','myhome','plastic','mars','dom','lb','ck','qi','crea','castle','amp','chart','serious','bridal','cancer','pars','risk',
       'italy','ava','ram','pulse','comm','driver','dis','alpine','mat','hz','vector','rhino','rep','trinity','sydney','ox','tango','shoes','boost',
       'fleet','ami','sip','pig','eve','tg','player','par','diva','resort','xtra','language','sap','working','radical','jumbo','cargo','disco','apt',
       'door','mybest','spain','ng','bangkok','muslim','tb','glam','renta','independent','eternal','bel','wii','ilike','mondo','member','disney','blind',
       'sample','shift','tk','tan','milliondollar','vermont','bro','cw','aurora','edit','crew','minnesota','moms','rf','plum','qatar','ark','dotcom','jh',
       'panama','turkey','cali','silk','advantage','articles','eq','maple','bud','entertainment','sanfrancisco','template','force','stars','baltimore',
       'memphis','cricket','brew','egypt','smash','yi','explore','jade','dt','mh','westcoast','always','used','whatis','quad','bass','manage','bl','casual',
       'vu','bestof','yy','cartoon','books','portable','rus','army','meeting','wb','ani','journey','progressive','stellar','pitch','cleveland','alta','fed',
       'morning','links','berlin','leaf','grupo','massage','inn','fluid','motorcycle','vp','inspired','myfree','oklahoma','tp','honest','cast','frame',
       'mouse','alive','mil','onestop','thinking','weekend','iheart','trail','mylife','catch','cart','prima','acu','daddy','newjersey','asset','cyprus',
       'wheel','copper','limo','strategy','offshore','nitro','lift','beijing','bloom','ass','arm','rb','healing','end','century','kwik','automotive',
       'findyour','inspire','course','peru','victoria','dh','which','silicon','skate','uno','neat','pinoy','canna','ergo','islam','boulder','halo',
       'res','jl','tower','rescue','adam','ada','cape','zee','dy','quiet','alter','reward','iv','gogreen','sen','scout','moo','hm','twisted','chris',
       'male','nexus','admin','puzzle','millionaire','sick','antique','tropical','dress','ye','cor','fabulous','attorney','ins','xp','recycle','zebra',
       'lit','getyour','hunter','dry','wizard','lp','leisure','alabama','sigma','sing','kw','blink','taste','wc','navi','jax','roof','evolution',
       'sushi','trusted','ama','hospital','getmy','phil','skinny','santa','photography','aw','forest','rockstar','api','dx','leather','midnight',
       'internetmarketing','underground','montreal','velo','pdf','adv','lottery','emerald','mother','engine','half','bw','status','eko','salsa','treasure',
       'alert','unity','illinois','booking','tactical','eazy','plane','primo','massive','bang','james','barter','follow','syn','muse','jt','qa','sql',
       'uv','smartphone','vehicle','juice','milk','cardio','coast','val','loud','smith','allthings','occupy','econo','madison','bayarea','cre','practice',
       'vino','eastern','campaign','kite','gplus','nav','zi','ob','bat','harmony','climate','arctic','grab','firm','joomla','mystery','rogue','bravo',
       'come','scott','lm','prestige','infini','holistic','multimedia','mental','chase','ft','smallbusiness','label','hao','columbus','zy','authentic',
       'kind','mai','birthday','df','bm','blogger','michael','kitty','lunch','del','report','flo','docu','amateur','gospel','horizon','lucid','idaho',
       'barcode','vis','gh','bicycle','publi','stupid','gw','babe','sole','alliance','qc','death','kentucky','youtube','gsm','sx','keen','joint','pl',
       'reg','nor','shirt','papa','retirement','avatar','ana','neu','survival','prop','spicy','rec','ren','patriot','wisconsin','camping','blackberry',
       'downtown','islamic','ly','leading','totally','battery','machine','linked','xpress','luck','elegant','sunrise','ju','cowboy','philadelphia','airport',
       'wx','exo','warrior','purchase','wei','genesis','aplus','kt','doodle','shutter','lightning','bath','cookie','cigar','early','omaha','duck','owl',
       'cz','boutique','fax','jackson','ima','zing','iweb','turtle','usb','ix','fact','lending','og','dw','incredible','heritage','homes','selling','igo',
       'tennessee','floor','mis','scrap','fivestar','allstar','mf','rex','windows','theperfect','das','cellular','police','hype','fone','epi','bin','lens',
       'sir','basketball','dfw','rice','paul','dolphin','northwest','ub','kidz','vg','calgary','cab','lonestar','ig','patient','bare','hop','webcam','fam',
       'pat','genuine','stl','steam','zo','welcome','angry','fr','shore','stealth','cut','elder','gateway','np','sierra','strange','magical','longisland',
       'heavy','tone','greatest','pride','maya','mirror','spectrum','mvp','aviation','groovy','ear','kr','psychic','nfl','taylor','thereal','level',
       'privacy','bluesky','ala','magazine','most','mysocial','poster','lava','mls','lt','yard','dino','sac','prof','pot','identity','gate','program',
       'bean','finger','avid','bf','fetish',
     ],

     domainSuffix:[
       'online','web','media','world','net','group','blog','shop','book','store','inc','tech','design','box','now','site','news','app','club','pro',
       'network','plus','tv','zone','link','co','cloud','it','life','works','city','hub','man','list','direct','info','mail','central','marketing','usa',
       'lab','house','solutions','me','art','music','host','home','market','guide','magazine','soft','center','games','land','live','software','space',
       'time','studio','one','talk','search','master','tube','point','apps','star','on','us','mobile','ware','com','cafe','spot','books','planet','mart',
       'line','power','in','place','video','card','forum','company','data','ville','source','page','today','mall','bank','team','press','pad','services',
       'view','corp','deals','biz','game','hq','express','work','service','max','bot','factory','mag','help','base','way','labs','board','nation','portal',
       'ly','systems','share','king','business','bar','map','cast','post','consulting','wire','jobs','project','free','travel','tools','vision','india',
       'watch','up','go','magic','code','bay','logic','trade','connect','digital','guru','finder','report','exchange','china','love','chat','smart','system',
       'social','directory','designs','tree','room','links','er','quest','port','show','mate','international','deal','wise','park','buzz','town','photo',
       'buy','ology','style','global','click','ing','kit','ads','uk','review','tool','hosting','products','gear','cash','expert','management','radio',
       'health','sales','server','sports','america','fun','law','school','people','stream','technology','money','stuff','support','care','wave','ad','day',
       'id','pages','log','pal','machine','station','pay','craft','stock','daily','girl','gallery','fire','track','find','mania','play','light','fx','dog',
       'cam','wiki','pedia','sale','ed','party','channel','tracker','print','depot','asia','job','is','solution','test','key','force','match','tips',
       'buddy','car','sites','insurance','domain','fly','gold','md','street','green','farm','reviews','mark','db','core','os','pack','academy','pod',
       'phone','feed','monkey','capital','mix','networks','plan','you','cards','training','agency','bee','events','wall','fox','guy','manager','ex','camp',
       'creative','monster','boy','top','gate','check','website','connection','masters','videos','engine','trader','builder','la','cat','file','first','pr',
       'all','sky','cube','productions','es','baby','garden','movie','tag','auto','doc','tour','partners','doctor','office','llc','pros','maker','ny',
       'rock','wear','index','hotel','kids','pool','sex','energy','university','geek','studios','sport','cn','fit','family','agent','times','brand','fix',
       'fish','research','fashion','pc','wizard','domains','easy','lounge','fast','local','journal','poker','control','mind','canada','iq','flow','tec',
       'blogs','technologies','photos','con','coach','story','nyc','face','seo','bid','community','safe','tek','rx','well','food','records','cart','ideas',
       'hunter','porn','films','fund','pop','advertising','golf','film','bag','girls','interactive','drive','alert','london','advisor','stop','notes','sense',
       'fitness','ink','age','square','at','supply','images','universe','shopping','photography','trading','band','vault','med','to','air','image','pass',
       'fusion','call','pictures','foryou','friend','centre','wiz','edge','fan','friends','graphics','circle','trip','idea','stars','desk','mob','partner',
       'dot','finance','maps','bus','io','guard','blue','fest','aid','pics','arts','outlet','access','experts','eye','homes','brain','science','realty',
       'sys','scape','zilla','hero','dev','best','date','path','auction','corner','pix','security','note','dream','by','togo','head','ss','internet','pub',
       'text','bridge','price','ventures','jet','scout','men','ok','plaza','sa','voice','gifts','lite','email','trust','focus','zoo','score','movies','zen',
       'cars','forums','case','vip','choice','ball','more','grid','event','topia','education','matrix','able','ster','ring','bug','village','set','casino',
       'beat','files','society','clothing','scan','spy','entertainment','zoom','broker','daddy','expo','gift','start','action','sonline','realestate','credit',
       'collection','seek','college','edu','win','factor','bit','rank','boss','tours','value','computer','concepts','sound','run','right','survey','class',
       'consultants','bet','here','resources','out','ltd','worx','boutique','fm','look','genius','storm','hut','traffic','consult','xpress','west',
       'construction','genie','information','insight','trends','island','ia','audio','coupons','bin','surf','toys','concept','motion','ist','flash','tel',
       'cool','az','bits','road','stores','paper','points','en','invest','swap','korea','rate','cell','commerce','crew','ee','stats','dr','hunt','jam',
       'area','cap','ip','medical','stone','loop','shops','water','communications','serve','as','japan','suite','speed','rewards','joy','earth','warehouse',
       'unlimited','ar','pic','trend','ninja','monitor','guides','lock','touch','sign','facts','worldwide','alliance','success','websites','form','advice',
       'robot','program','creations','europe','docs','select','property','beauty','library','spark','dating','kid','heaven','projects','promo','name','sync',
       're','xchange','brands','sun','ace','shoes','foundation','models','ready','coupon','hd','quick','wood','angel','model','webdesign','dc','fair','sms',
       'resource','hotels','do','development','revolution','red','smith','quote','avenue','saver','chicago','pulse','discount','ca','loan','mode','update',
       'africa','globe','next','therapy','tip','ondemand','jewelry','im','candy','bird','diet','ify','inn','mine','save','rocket','country','blast','blogger',
       'guys','ez','simple','content','culture','shot','diary','forlife','spa','ship','tap','designer','wine','fuel','train','this','publishing','for','bids',
       'tunes','articles','sell','union','good','hot','enterprises','war','clinic','ol','junction','cycle','sphere','properties','an','boards','reports','ms',
       'assist','study','dance','production','can','furniture','learning','oo','lane','quotes','field','gaming','shack','shark','financial','thai','squad','tab',
       'deck','bags','xxx','shopper','matic','tax','fans','database','legal','parts','back','living','god','snap','talent','plex','promotions','apparel','al',
       'st','cc','spring','less','scene','mom','bucks','block','storage','arena','workshop','week','new','industries','word','builders','clip','coffee','dom',
       'institute','history','tone','scope','corporation','flex','leads','dreams','newyork','analytics','webs','pot','snow','signs','player','engineering',
       'tickets','fresh','chart','artist','awards','stories','australia','mac','metrics','pilot','mo','associates','vine','mill','wow','gen','screen','lady',
       'ix','supplies','valley','pet','berry','state','only','tees','vu','secrets','republic','strategy','ma','offers','or','lead','leader','shirts','loans',
       'wireless','united','clicks','venture','ic','clean','dynamics','groups','advantage','cms','pin','jack','empire','ism','panel','side','lawyer','geeks',
       'helper','ie','hit','le','lines','pa','printing','connections','offer','queen','forge','tags','zine','solar','dude','wars','boom','galaxy','challenge',
       'rack','happy','coaching','logistics','download','secure','zero','den','hr','menu','names','inside','door','byte','consultant','table','investments',
       'seeker','bio','planner','juice','rentals','folio','haven','crowd','crazy','writer','auctions','ya','speak','sea','archive','lover','comm','experience',
       'script','catalog','marketplace','calendar','am','insider','like','answers','boost','order','bytes','yard','future','hound','haus','chef','tweet','frog',
       'comp','frame','ers','drop','ticket','matters','words','prints','sol','dealer','dna','creation','tutor','options','computers','rs','hack','forme','elite',
       'clips','son','think','flix','knowledge','led','kitchen','mi','rush','river','eo','locator','boys','load','dogs','li','ent','bd','glass','runner','color',
       'safety','freak','oil','stocks','em','industry','junkie','et','savings','things','miami','meet','enterprise','profit','conference','dvd','rental','hire',
       'bill','sf','soup','goods','salon','rus','mates','abc','shots','zip','performance','era','real','crm','cams','promotion','bike','toy','rent',
       'song','shield','change','admin','plans','my','trax','forever','vote','tank','truck','codes','see','perfect','paradise','heart','made','tribe',
       'demo','advisors','innovation','racing','results','down','cs','hk','product','bc','droid','bingo','bear','hop','ro','icon','bazaar','spin','lot',
       'open','weekly','innovations','cup','five','ride','authority','wealth','off','nut','foto','type','profile','rocks','url','delivery','professional',
       'forsale','cover','pick','kings','lists','mojo','brokers','collective','so','walk','um','views','texas','si','dir','trainer','active',
       'electronics','kits','bible','build','boston','markets','locker','ys','race','sight','stack','bg','bucket','coop','career','savvy','yoga','hive',
       'adventure','gps','tea','tastic','doctors','wholesale','pak','digest','explorer','funds','let','trail','women','electric','hall','staff','creator',
       'xl','vibe','yes','window','gadget','skin','act','tex','listings','shirt','platform','fu','nexus','radar','nest','alerts','boat','careers',
       'retail','pets','tracks','pen','registry','campus','buyer','mortgage','rich','dash','filter','freedom','foods','sinc','mama','garage','int',
       'lovers','makers','mountain','ts','pie','graph','picture','op','themes','ba','reader','bb','serv','move','se','keeper','hawaii','loft','dock',
       'logo','camera','secret','mexico','stage','ac','get','atlanta','hair','television','ways','soul','step','days','lights','forms','strategies',
       'vegas','diva','meter','eco','stat','sage','ra','pals','sworld','kingdom','adventures','hat','hill','sy','mint','sc','body','ice','ratings',
       'equipment','forex','tweets','clouds','prime','ness','register','specialist','ec','mentor','addict','away','ability','anywhere','investment',
       'bydesign','thailand','servers','rating','portfolio','angels','pipe','ops','rep','woman','firm','merchant','ta','booth','intelligence','special',
       'prices','discounts','summit','mobi','course','break','spirit','skills','how','rules','florida','wheel','thing','math','profits','basket','learn',
       'spots','giant','english','listing','apple','vista','etc','eyes','nova','ins','vox','ii','athome','clubs','high','stand','repair','flip','two',
       'big','record','festival','dns','ea','trek','hits','forall','seed','flight','mc','theory','front','communication','mash','roll','agents','contact',
       'compare','companies','ai','vid','cd','passion','bargains','el','mx','dental','restaurant','pt','songs','evolution','va','quiz','traders','bots',
       'night','driver','ease','alarm','scoop','dj','status','wisdom','palace','poll','lux','bright','ap','bbs','sure','gateway','pixel','houston','nerd',
       'jump','classifieds','brothers','clock','dex','cheap','ko','cut','taxi','pi','seller','ful','france','lifestyle','browser','fax','analysis',
       'protection','ray','booking','beach','copy','mat','xp','hawk','fever','lighting','lift','picks','buys','chic','gym','villa','wellness','ir',
       'mediagroup','chain','plant','wind','bud','cruise','bomb','centric','quality','tx','route','south','gram','building','series','programs','gamer',
       'tr','meeting','holdings','boxes','dragon','tower','ireland','magnet','display','switch','brasil','income','tiger','impact','medic','motor','beer',
       'bliss','places','comics','vids','be','tracking','sim','effect','talks','nj','gadgets','ten','keys','essentials','shows','moon','label','ocean',
       'charts','flower','wing','marks','cats','target','oc','nz','motors','accessories','cake','pac','ave','de','dad','trips','austin','intl','gal',
       'na','brazil','bond','buddies','stick','holiday','flowers','av','forward','sounds','shift','pig','proxy','concierge','too','arcade','shoppe',
       'trans','zap','hacker','maven','cook','deluxe','crunch','forest','wallet','contest','nutrition','teacher','werks','cinema','graphic','ps','verse',
       'api','castle','church','gator','extreme','together','peak','unit','jungle','clear','assistant','alive','champ','launch','patrol','hi','and','ri',
       'inabox','basics','consultancy','barn','of','smile','cow','wedding','styles','six','google','faq','sat','tune','hood','bite','viewer','silver',
       'casting','reality','techs','vn','proof','hand','ski','pos','van','user','dubai','schools','ds','vacation','org','kick','spaces','lens','warrior',
       'couture','minds','full','professionals','bux','italia','seven','oz','root','bound','italy','ns','paint','andco','dollars','opolis','chip','eng',
       'bikes','pizza','affiliate','tron','trak','om','architecture','trac','ag','qa','league','turkey','superstore','diy','logix','football','about','cal',
       'write','guild','soccer','forkids','da','ebook','downloads','nc','wish','black','bell','sd','tablet','spain','phones','karma','heads','cop','ling',
       'decor','north','nuts','compass','telecom','gas','museum','dates','andmore','clothes','checker','travels','protect','gem','bo','chick','forless',
       'healthcare','army','feeds','poster','waves','finders','sources','srus','safari','that','planning','rain','atlas','facebook','dollar','webhosting',
       'phoenix','prep','tee','cave','machines','testing','snet','california','rooms','recipes','gems','spider','peru','ry','wolf','holidays','bob','rider',
       'limited','backup','bi','leaf','ct','estate','better','linx','squared','widget','muse','button','reach','journey','medicine','butler','ni','tronics',
       'lo','ty','roulette','distribution','generation','pit','lease','tales','visions','yo','sblog','dd','fact','omatic','part','tix','jazz','beats','caster',
       'funding','truth','whiz','ant','ox','sheet','gurus','coin','booster','account','vue','east','mouse','ter','crafts','networking','edit','fab','nic','ranch',
       'shine','extra','method','lawyers','ask','lasvegas','sweb','fight','wrap','bars','jp','dome','pond','horse','babes','xs','bang','patch','scale','reporter',
       'transport','turk','architects','metal','audit','pm','forfree','sharing','ton','answer','fuse','wap','rite','sos','cab','scripts','alley','intel','branding',
       'ka','qr','ng','create','ground','windows','recovery','theme','fone','questions','marine','bubble','ti','investor','posters','canvas','buster','ion','hope',
       'lion','surfer','no','wheels','send','panda','bargain','insights','ku','il','plug','rescue','za','houses','surveys','ish','un','discovery','ne','span','mon',
       'printer','templates','benefits','choices','podcast','hunters','rates','count','read','blaster','beta','trace','dallas','watches','mod','kiosk','inmotion',
       'atwork','virtual','resort','watcher','battle','foundry','multimedia','minder','promos','licious','onweb','artists','bros','fy','rc','tc','skill','level','ce',
       'bowl','toolbox','trades','toronto','savers','wines','shares','zz','critic','leaders','joe','cache','pocket','centers','wide','transfer','strong','cross','bed',
       'ping','wild','mad','coder','bonus','hotline','message','collector','mirror','weather','clan','attorney','meister','res','bites','architect','diamond','ga',
       'mission','gourmet','lee','sec','paris','newsletter','attack','cable','package','node','counter','banking','navigator','designers','detective','sandiego',
       'computing','sp','sexy','worker','ch','pharma','ur','fishing','supermarket','payment','mailer','groove','smarts','provider','bull','orama','freaks','autos',
       'scribe','worlds','ler','automation','ceo','circles','steps','memory','award','navi','heroes','painting','cpa','sdirect','shed','sin','hour','vietnam','hook',
       'tshirts','tronic','walker','premium','beam','everything','br','gang','will','wings','owl','seal','micro','yourself','gay','four','hardware','emporium','scanner',
       'messenger','generator','ies','babe','kart','mafia','franchise','sh','lan','tt','teen','ity','kc','foot','sup','say','letter','nets','task','nine','seattle',
       'theater','corps','dig','dotcom','add','js','su','make','nature','cost','disk','heat','blocks','process','please','vacations','singles','dress','xpert','aware',
       'tones','student','calls','around','cad','metro','hacks','bunny','metrix','synergy','pk','underground','police','tricks','wizards','recruitment','mm','fantasy',
       'court','meme','splus','equity','lib','mobility','massage','engineer','rex','iran','memo','gun','floor','aa','mesh','geo','interiors','accounting','lake','insure',
       'pads','over','sgroup','location','showcase','ru','again','bands','winner','fruit','fi','socialmedia','parking','super','updates','its','harmony','lynx','lv',
       'cleaning','we','inbox','wa','prod','fin','zy','ian','organic','worth','bureau','jar','susa','singapore','www','option','logos','theatre','aholic','sonic','arch',
       'zones','lodge','ak','specialists','kiss','manual','ography','orlando','madness','ize','specials','bets','sg','laser','venue','balance','hosts','director','ngo',
       'child','total','ey','busters','complete','gig','jeans','farms','tape','toyou','losangeles','egg','goal','ium','colorado','skins','ky','est','oasis','scapes',
       'miles','folder','album','txt','incorporated','pump','brew','tattoo','voip','dashboard','matter','streaming','push','developer','risk','cv','ark','hype','onthego',
       'bench','bean','te','rose','au','payments','applications','cure','blitz','upload','starter','signal','lessons','ax','catering','charge','bling','defense','grade',
       'faces','relief','android','logs','flyer','affiliates','organizer','chatter','imaging','ho','import','plate','professor','sourcing','dial','cr','arabia','shelf',
       'moms','duck','formula','disc','vc','sclub','orange','streams','commercial','beast','sbook','movement','labels','steel','ontheweb','application','twitter','cities',
       'values','pink','end','login','iphone','jo','fa','nepal','lord','champion','ab','sugar','drink','classic','cases','profiles','courses','reference','dish','balls',
       'bazar','shoe','designstudio','thoughts','visual','tm','reward','luxury','rat','magazines','fl','gain','manage','tom','adviser','pride','vest','packs','surfing',
       'via','supplier','shell','moto','utah','staffing','ebooks','fairy','lotto','lit','number','fare','maniac','banks','selling','moves','association','grow','pharmacy',
       'feedback','od','mixer','movers','splash','pixels','american','swag','crush','axis','writers','kin','resume','tell','burger','denver','monkeys','fs','vendor','luck',
       'where','elements','sit','punch','banner','public','ally','chase','casa','article','devil','pan','nice','lex','assets','teens','mex','with','dictionary','template',
       'checks','porno','stamp','coins','ha','bro','opia','posts','rabbit','egypt','chips','draw','puppy','mass','pon','ls','companion','editor','politics','rise','cove',
       'vibes','threads','hopper','white','hell','slot','ay','spec','needs','preview','wash','sweet','craze','junkies','reg','cy','nt','charter','advance','vet','small',
       'worthy','flicks','papers','stickers','beyond','homepage','rap','frenzy','parties','arc','collections','limo','tennis','fortune','rev','colors','publications','ci',
       'response','chicks','connector','lottery','writing','great','detroit','fabric','ranking','perks','delight','animation','maxx','recipe','tops','chem','friendly',
       'cargo','adv','cosmetics','credits','xx','brains','traveler','budget','punk','who','rec','junky','fail','caddy','prize','covers','buyers','ui','mt','rr','blogging',
       'mgmt','selection','armor','ohio','1','monsters','contacts','ux','deli','vancouver',
     ],

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
      // when first  input fields have some data
      else {
        for(let i = 0; i < this.firstTextArr.length; i++)
        {
            combos.push(this.firstTextArr[i])
        }
      }
      return combos;
    }
  },
  methods:{
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
