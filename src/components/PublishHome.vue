<template>
    <div>
        <div class="gradient topBox" :style="{ height: screenHeight + 'px' }">
            <div>
                <h1>What would you like to embed?</h1>
                <form action="">
                    <input type="text" v-model="url" class="inpQuery">
                    <button @click.prevent="createLink()">
                        go
                    </button>
                </form>
                <a href="#options" class="ch_link_white" rel="noopener noreferrer">Or browse your options below</a>
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
        <div v-if="type">
            <EmbedFrame :body="embedHtml"></EmbedFrame>                        
        </div>
        <template v-else>
            <EmbedBox></EmbedBox>
        </template>
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
            url: null,
            targetDOMs:null,
            embeddable:null,
            embedHtml:null,
            type:null,
        };
    },
    mounted() {
        this.screenHeight = window.innerHeight - 150;
        window.addEventListener('resize', this.updateScreenHeight);
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.updateScreenHeight);
    },
    methods: {
        updateScreenHeight() {
        this.screenHeight = window.innerHeight - 150;
        },
        findTargets() {
            this.targetDOMs = document.querySelectorAll('.' + this.options.target);
        },
        createLink() {
            if(this.url === null){
                alert(1231);
                return;
            }
            var u = this.url.split('//');
            if (u[0] === "http:" || u[0] === "https:") {
                var keys = u[1].split('/');
                if (keys[0] === "cubehall.com") {
                    console.log("cub");
                    if (this.validateIdentifier(keys[1], this.identifiers)) {
                        this.embeddable = this.domain + '/embed/' + keys[1] + "/" + keys[2];
                        this.type = keys[1];
                        
                    } else {
                        this.embeddable = this.domain + '/embed/store/' + keys[1];
                        this.type = "store";
                    }
                    
                    this.getIframe(this.embeddable);
                } else {
                    alert(1);
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
            })
            .catch(error => {
                console.error(error);
            });
            
        },
        createFrame() {
            this.findTargets();

            this.targetDOMs.forEach(e => {
                var l = e.querySelector(this.permalink).getAttribute('href');
                console.log(l);
                e.querySelector(this.permalink).setAttribute("class", "cubehall-btn-link");
                if (this.validateUrl(l)) {
                    this.getIframe(this.createLink(l))
                    .then(data => {
                        this.embed(e, data);
                    });
                }
                this.resizeFrame(e);
            });
            this.insertStylesheet();
        },
        embed(e, data) {
            e.insertAdjacentHTML('afterbegin', data);
        },
        resizeFrame(e) {
            var w;
            w = e.getAttribute('width');
            if (w) {
            e.style.maxWidth = w;
            } else {
            e.style.maxWidth = "100%";
            }
        },
        insertStylesheet() {
            var style = "@import url(https://fonts.googleapis.com/css?family=Nunito); @import url(https://fonts.googleapis.com/css2?family=Lato:wght@100;300;400;700;900&display=swap); @charset 'UTF-8'; .cubehall-embed-block { border-radius: 24px; overflow: hidden; border:solid 1px #c8c8c8; } .cubehall-btn-link { margin: 1px; color:black!important; text-decoration:underline; display: flex; } .cubehall-embed-img { width:100%; } .cubehall-embed-logo { height:auto; width:auto; max-height:28px; max-width:28px; } [data-type='cubehall-link'] { display: flex; justify-content:space-between; margin:16px; } .cubehall-store-wrap { display: grid; grid-template-columns: 25% 25% 25% 25%; } .cubehall-embed-profile-info { display:flex; } .cubehall-embed-profile-pic { height:48px; width:48px; border-radius:50%; overflow:hidden; flex-shrink:0; } .cubehall-embed-profile-pic img { height:100%; width:100%; }";

            var css = document.createElement('style');
            css.type = 'text/css';

            if(css.styleSheet){
                css.styleSheet.cssText = style;
            }
            else
            {
                css.appendChild(document.createTextNode(style));
            }

            document.getElementsByTagName("head")[0].appendChild(css);
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
  