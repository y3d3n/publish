<template>
    <div>
        <NoticeBox ref="messageBox"></NoticeBox>
        <div class="gradient topBox" :style="{ height: screenHeight + 'px' }">
            <div :style="{ marginTop: scrollPosition + 'px' }">
                <h1 class="_ch_head">What would you like to embed?</h1>
                <form class="_frm">
                    <input type="text" @focus="isInputActive = true"  placeholder="Enter a CubeHall URL here" v-model="url" class="inpQuery">
                    <button @click.prevent="createLink()" class="_go"> 
                        <svg  width="24px" height="24px" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path d="M16.707,18.707a1,1,0,0,1-1.414-1.414L19.586,13H2a1,1,0,0,1,0-2H19.586L15.293,6.707a1,1,0,0,1,1.414-1.414l6,6a1,1,0,0,1,0,1.414Z"/>
                        </svg>
                    </button>
                </form>
                <div 
                    v-show="isInputActive" 
                    ref="menu"
                    class="_ch_suggest">
                    <div @click="changeUrl(embed.product)" class="sg_card">
                        A Product: <span class="sg_link">{{embed.product}}</span>
                    </div>
                    <div @click="changeUrl(embed.parlor)" class="sg_card">
                        A Parlor <span class="sg_link">{{ embed.parlor }}</span>
                    </div>
                    <div @click="changeUrl(embed.studio)" class="sg_card">
                        A Studio <span class="sg_link">{{ embed.studio }}</span>
                    </div>
                    <div @click="changeUrl(embed.collection)" class="sg_card">
                        A Collection <span class="sg_link">{{ embed.collection }}</span>
                    </div>
                    <div @click="changeUrl(embed.showoff)" class="sg_card">
                        A ShowOff <span class="sg_link">{{ embed.showoff }}</span>
                    </div>
                </div>
            </div>
            <div class="nav">
                <a :href="thisSite" class="ch_lg_holder nav_link">
                    <img class='ch_logo' src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAvCAYAAAClgknJAAAACXBIWXMAAAsSAAALEgHS3X78AAACXElEQVRoge1a4VHCMBh9eP6nTgBOAExgNxAnoG7ABrqBbCBugBMIG8AGZQLrBJ8X7/vOGJOmpG1o73h3AUrT9r0m+b6XNiCiqiUjoi11A2siShTvgfrwIAGwBTDxVYyMHYDUJ6Cr5AWz65KdNvIHAMs43Jyc1gCGsu0S4CKfAigiEC3DHsCd7L+yVOwy+X8wBfSKPAwBvSMPTUAvyUMT0EvyYAHPfSUPFpBq270ir2DmAUV8GpnDts7BSkCuJQb1/dEMr8o4AphzgjoZMga+IpPWMeJWSEIF5Nxt3luj6IfyNlnIgRJGc27GQcRyw91HENwC54IKGHnda7vc6NRyR4rQgdYmdAEJe/2l5rdteOOBX/vuNQHTCz15yCssuCVi5wsrXF7IhyEfM+6CgMzihWaWaHRrhNohd6Wz4orDp+DIXsg2WCXU6iIWXRCgR5t1BSNn3vXUUS8KQvJAp0LpORNZI7gIODcuAgCsQp1kEwgVcNB+qyT4CYAsZc9htzWBoQKqZuAJ+6vWvFOogA2AhxOmoqO2vFPZ43UfNtw1XJlY7Lk8MBjyeJlXO70Tf7pjHQGCssciG7Yn4pnuuRVC5xKm8cxjhFHzhUhoN1LkX7XtXSwBhTF5D4FJHhJIYiWyOtNPG/lH6bpdz8Qu8mvZiCUgpN97ySOSgITzwCmoRD6WADMK+cZDZfKw5IElp/2m3g9kxrx5VyIg5eu/GP87yf+AiFaRVj8URDQ11mX4rp351nGAF00UEcjPLQTGJdf2khcBIkKtAGmDuDqvIuoioVplo9VXrVJW/7cQ4RtSEDPbV6MsaAAAAABJRU5ErkJggg=='/>
                    <span class="lg_txt">
                        / publish
                    </span>                    
                </a>
                <div>
                    <a  class="nav_link" href="https://cubehall.com" target="_blank" rel="noopener noreferrer">Visit CubeHall</a>
                    
                </div>
            </div>
        </div>
        <div class="_frm_hdr" :style="{ height: screenHeight + 'px' }">
            <template v-if="type">
                <EmbedFrame :body="embedHtml" :embed="cleanedUrl" :type="type"></EmbedFrame>
            </template>
            <template v-else>
                <EmbedBox></EmbedBox>
            </template>
                <CommonQuestions></CommonQuestions>
        </div>
        
    </div>
  </template>
  
  <script>
    import EmbedBox from './EmbedBox.vue';
    import EmbedFrame from './EmbedFrame.vue';
    import CommonQuestions from './CommonQuestions.vue';
    import NoticeBox from './NoticeBox.vue';
    import axios from 'axios';

  export default {
    name: 'PublishHome',
    props: {
      msg: String
    },
    components: {
        EmbedBox, EmbedFrame, CommonQuestions, NoticeBox
    },
    data() {
        return {
            options : {
                target: "cubehall-embed-block",
                showMedia: true,
                showTitle: true,
                showDescription: false,
                _blank: false,
                showButton: false
            },
            embed:{
                product: "https://cubehall.com/list/chadke",
                parlor: "https://cubehall.com/parlor/guide-on-hosting-parlor",
                studio: "https://cubehall.com/cubehall",
                collection: "https://cubehall.com/board/4biz_udv0",
                showoff: "https://cubehall.com/showoff/d14t05rbk3gu"
            },
            target: "cubehall-embed-block",
            screenHeight: null,
            domain: "http://192.168.1.66/api",
            permalink : "a[data-link]",
            message: "Sorry, we couldn't understand that query. Please try again.",
            identifiers: ['list', 'showoff', 'board', 'follow', "parlor"],
            url: "",
            embeddable:null,
            embedHtml:"",
            type:null,
            cleanedUrl:"",
            scrollPosition:0,
            thisSite:"",
            isInputActive: false
        };
    },
    mounted() {
        this.screenHeight = window.innerHeight - 150;
        this.scrollPosition =  window.scrollY;
        window.addEventListener('resize', this.updateScreenHeight);
        window.addEventListener('scroll', this.handleScroll);

        // for menu
        document.addEventListener('mousedown', this.handleClickOutside);
    },
    created(){
        // const currentUrl = window.location.href;
        this.thisSite = window.location.protocol + "//" + window.location.hostname;
        console.log(encodeURIComponent(window.location.hostname));
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.updateScreenHeight);
        window.removeEventListener('scroll', this.handleScroll);
        document.removeEventListener('mousedown', this.handleClickOutside);
    },
    methods: {
        handleClickOutside(event) {
            const menu = this.$refs.menu
            if (menu && !menu.contains(event.target)) {
                this.isInputActive = false;
            }
        },
        changeUrl(url){
            this.url = url;
            this.isInputActive = false;
        },
        updateScreenHeight() {
            this.screenHeight = window.innerHeight - 150;
        },
        handleScroll() {
            this.scrollPosition = window.scrollY;
        },
        checkUrl() {
            // Regular expression pattern to match against the URL
            const urlPattern = /^(http|https):\/\/[^ "]+$/;

            // Check if the URL matches the pattern
            return urlPattern.test(this.url);
        },
        createLink() {
            if(this.url === null){
                this.$refs.messageBox.message = "Sorry, we couldn't understand that query. Please try again.";
                return;
            }
            if(!this.checkUrl()){
                this.$refs.messageBox.message = "Sorry, we couldn't understand that query. Please try again.";
                return;
            }else{
                if (this.url.indexOf('?') !== -1) {
                    this.cleanedUrl = this.url.substring(0, this.url.indexOf('?'));
                } else {
                    this.cleanedUrl = this.url;
                }
            }
            
            var u = this.cleanedUrl.split('//');
            if (u[0] === "http:" || u[0] === "https:") {
                var keys = u[1].split('/');
                if (keys[0] === "cubehall.com") {
                    if (this.validateIdentifier(keys[1], this.identifiers)) {
                        this.embeddable = this.domain + '/embed/' + keys[1] + "/" + keys[2];
                        this.type = keys[1];
                    } else {
                        this.embeddable = this.domain + '/embed/store/' + keys[1];
                        this.type = "store";
                    }
                    
                    this.getIframe(this.embeddable);
                } else {
                    this.$refs.messageBox.message = "Sorry, we couldn't understand that query. Please try again.";
                    return;
                }
            }else{
                this.$refs.messageBox.message = "Sorry, we couldn't understand that query. Please try again.";
                return;
            }
        },
        validateIdentifier(id, identifiers) {
            return (identifiers.indexOf(id) > -1);
        },
        async getIframe(url = "") {
            await axios.get(url)
            .then(response => {
                this.embedHtml = response.data;
                this.scrollToEmbed();
            })
            .catch(error => {
                console.error(error);
            });
            
        },
        scrollToEmbed(){
            window.scrollTo({
            top: this.screenHeight,
            behavior: 'smooth'
            });
        }
        
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
    ._ch_head{
        font-weight: 100;
        font-size: 38px;
        margin: 0 0 15px;
    }
    ._ch_suggest{
        position: absolute;
        background: white;
        color: #000000;
        width: max-content;
        text-align: left;
    }
    .sg_card{
        padding: 15px;
        cursor: pointer;
        font-size: 14px;
    }
    .sg_link{
        color: #f64a6d;
        font-weight: 600;
    }
    .sg_card:hover{
        background: #e6cccc;
    }
    .gradient{
        background: linear-gradient(45deg,#f64a6d,#ca399a);
        color:white;
    }
    .topBox{
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 0 15px;
        box-sizing: border-box;
        overflow: hidden;
    }
    ._frm_hdr{
        min-height: 500px;
        padding: 15px;
        box-sizing: border-box;
    }
    ._frm{
        display: flex;
        position: relative;
    }
    ._go{
        position:absolute;
        height:100%;
        right: 0;
        cursor: pointer;
        background: none;
        border: none;
        margin-right: 10px;
    }
    ._go svg{
        fill:#747474;
    }
    ._go svg:hover{
        fill:#000000;
    }
    .nav_link{
        color: white;

    }
    .nav{
        position: absolute;
        top: 0;
        max-width: 1200px;
        width: 100%;
        display: flex;
        justify-content: space-between;
        padding: 15px;
        box-sizing: border-box;
    }
    .ch_logo{
        height: 24px;
        width: 24px;
        margin-right: 8px;
    }
    .ch_lg_holder{
        display: flex;
        flex-direction: row;
        align-items: center;
    }
    .ch_link_white{
        text-decoration: none;
        color: white;
        margin: 15px 0;
        display: block;
    }
    .lg_txt{
        padding: 0;
        margin: 0;
    }
    .inpQuery{
        box-sizing: border-box;
        width: 100%;
        padding: 15px 36px 15px 20px;
        border: 0;
        border-radius: 4px;
        background: #fff;
        color: #292f33;
    }
    div{
        margin: 0;
    }
  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  </style>
  