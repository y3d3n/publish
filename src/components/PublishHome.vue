<template>
    <div>
        <div class="gradient topBox" :style="{ height: screenHeight + 'px' }">
            <div :style="{ marginTop: scrollPosition + 'px' }">
                <h1>What would you like to embed?</h1>
                <form action="">
                    <input type="text" v-model="url" class="inpQuery">
                    <button @click.prevent="createLink()">
                        go
                    </button>
                </form>
            </div>
            <div class="nav">
                <div class="ch_lg_holder">
                    <img class='ch_logo' src='data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAvCAYAAAClgknJAAAACXBIWXMAAAsSAAALEgHS3X78AAACXElEQVRoge1a4VHCMBh9eP6nTgBOAExgNxAnoG7ABrqBbCBugBMIG8AGZQLrBJ8X7/vOGJOmpG1o73h3AUrT9r0m+b6XNiCiqiUjoi11A2siShTvgfrwIAGwBTDxVYyMHYDUJ6Cr5AWz65KdNvIHAMs43Jyc1gCGsu0S4CKfAigiEC3DHsCd7L+yVOwy+X8wBfSKPAwBvSMPTUAvyUMT0EvyYAHPfSUPFpBq270ir2DmAUV8GpnDts7BSkCuJQb1/dEMr8o4AphzgjoZMga+IpPWMeJWSEIF5Nxt3luj6IfyNlnIgRJGc27GQcRyw91HENwC54IKGHnda7vc6NRyR4rQgdYmdAEJe/2l5rdteOOBX/vuNQHTCz15yCssuCVi5wsrXF7IhyEfM+6CgMzihWaWaHRrhNohd6Wz4orDp+DIXsg2WCXU6iIWXRCgR5t1BSNn3vXUUS8KQvJAp0LpORNZI7gIODcuAgCsQp1kEwgVcNB+qyT4CYAsZc9htzWBoQKqZuAJ+6vWvFOogA2AhxOmoqO2vFPZ43UfNtw1XJlY7Lk8MBjyeJlXO70Tf7pjHQGCssciG7Yn4pnuuRVC5xKm8cxjhFHzhUhoN1LkX7XtXSwBhTF5D4FJHhJIYiWyOtNPG/lH6bpdz8Qu8mvZiCUgpN97ySOSgITzwCmoRD6WADMK+cZDZfKw5IElp/2m3g9kxrx5VyIg5eu/GP87yf+AiFaRVj8URDQ11mX4rp351nGAF00UEcjPLQTGJdf2khcBIkKtAGmDuDqvIuoioVplo9VXrVJW/7cQ4RtSEDPbV6MsaAAAAABJRU5ErkJggg=='/>
                    <span class="lg_txt">
                        / publish
                    </span>                    
                    </div>
                <div>
                    visit cubehall
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
        </div>
        
    </div>
  </template>
  
  <script>
    import EmbedBox from './EmbedBox.vue';
    import EmbedFrame from './EmbedFrame.vue';
    import axios from 'axios';

  export default {
    name: 'PublishHome',
    props: {
      msg: String
    },
    components: {
        EmbedBox, EmbedFrame
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
            target: "cubehall-embed-block",
            screenHeight: null,
            domain: "http://192.168.1.68/api",
            permalink : "a[data-link]",
            messages: {
                missingTargetid: "Please specify element target id",
                missingTargetHtml: "HTML element with target class not fould",
                missingFeedUrl: "Embedable URL not found",
                errorFetching: "Error reading data from remote url"
                },
            identifiers: ['list', 'showoff', 'board', 'follow', "parlor"],
            url: "",
            embeddable:null,
            embedHtml:"",
            type:null,
            cleanedUrl:"",
            scrollPosition:0
        };
    },
    mounted() {
        this.screenHeight = window.innerHeight - 150;
        this.scrollPosition = window.scrollY;
        window.addEventListener('resize', this.updateScreenHeight);
        window.addEventListener('scroll', this.handleScroll);
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.updateScreenHeight);
        window.removeEventListener('scroll', this.handleScroll);
    },
    methods: {
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
                alert("Something's Not Right. Sorry, we couldn't understand that query. Please try again.");
                return;
            }
            if(!this.checkUrl()){
                alert("Something's Not Right. Sorry, we couldn't understand that query. Please try again.");
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
                    alert("Something's Not Right. Sorry, we couldn't understand that query. Please try again.");
                    return;
                }
            }else{
                alert("Something's Not Right. Sorry, we couldn't understand that query. Please try again.");
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
  