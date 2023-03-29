<template>
    <div height="500px">
        <div class="embMsg">
            <div>
                <h2 class="_ch_txt_h1">That’s all we need, unless you’d like to <span @click="activeCustomize" class="text_link">set customization options</span>.</h2>
            </div>
            <div v-if="enableCustomize" class="_ch_custon">
                <div>
                    <div class="_ch_gray">
                        What size would you like your {{type}} to be?
                    </div>
                    <div class="_ch_sm_m">
                        <input class="_ch_slider" type="range" min="100" max="1300" v-model="width" @input="updateValue" />
                    </div>
                    <div class="_ch_sm_m">
                        <input class="_ch_c_inp" type="number" name="" id="" v-model="width">
                    </div>

                </div>
                
                
            </div>
            <div style="position:relative" class="embCode-content">
                <code type="text" class="EmbedCode">
                    {{ customMessage }}
                    </code>
                <button @click.prevent="copyTextToClipboard" class="_copy">Copy Code</button>
            </div>
        </div>
        <div>
            <div class="_ch_gray">Preview</div>
            <div class="embBody">
                <div :style="{ width: width + 'px' }" class="embCard">
                    <div v-html="body"></div>
                </div>

            </div>
            
        </div>
        
    </div>  
</template>

<script>
export default {
    data(){
        return{
            toClip:"",
            canClip:false,
            width: 400,
            height:300,
            enableCustomize: false,
            boxWidth: 150,
        }
    },
    props:['body', 'embed', 'type'],
    methods:{
        copyTextToClipboard() {           
            this.toClip = `<div class="cubehall-embed-block" data-view="${this.type}" width="${this.width}px" height="${this.height}px"><a href="${this.embed}" target="_blank" data-link>View On CubeHall</a></div>`;

            navigator.clipboard.writeText(this.toClip)
                .then(() => {
                console.log('Text copied to clipboard');
                })
                .catch(error => {
                console.error('Error copying text: ', error);
            });
        },
        activeCustomize(){
            this.enableCustomize = true;
        }
       
    },
    computed:{
        customMessage() {
            return `<div class="cubehall-embed-block" data-view="${this.type}" width="${this.width}px" height="${this.height}px"><a href="${this.embed}" target="_blank" data-link>View On CubeHall</a></div>`;
        }
    },

}
</script>

<style>
.embMsg{
    display: flex;
    width: 100%;
    padding: 15px;
    flex-direction: column;
    box-sizing: border-box;
}
.embCode-content{
    padding: 15px 0;
    margin: 0 auto;
    display: flex;
    box-sizing: border-box;
    width: 100%;
    max-width: 500px;
}
._copy{
    background: #d82448;
    border: none;
    color: white;
    padding: 0 15px;
    white-space: nowrap;
    border-radius: 0 6px 6px 0;
}
.text_link{
    color:#d82448;
    cursor: pointer;
}
._ch_txt_h1{
    font-weight: 200;
    font-size: 21px;
}
.text_link:hover{
    text-decoration: underline;
}
._ch_custon{
    width: 100%;
    display: flex;
    justify-content: center;
    padding: 25px 6px;
    box-sizing: border-box;
}
._ch_sm_m{
    margin: 15px 0;
}
._ch_c_inp{
    padding: 8px 15px;
}
._ch_gray{
    color:#8899a6
    
}
.EmbedCode {
    display: table-cell;
    white-space: nowrap;
    top: 0;
    color: #8899a6;
    padding: 8px 3px 8px 15px;
    overflow: hidden;
    display: block;
    text-overflow: ellipsis;
    box-shadow: inset 1px 1px 2px 0 rgba(0,0,0,.1);
    border-radius: 4px 0 0 4px;
    border: 1px solid #ccd6dd;
    border-width: 1px 0 1px 1px;
    overflow: hidden;
    width: 100%;
    position:relative;

}
.embBody{
    width: 100%;
    display: flex;
    justify-content: center;
    background: rgba(239,239,239,.5);
    min-height: 150px;
    padding: 25px;
    box-sizing: border-box;
}
.embCard{
    border-radius: 24px;
    overflow: hidden;
    border: solid 1px #c8c8c8;
}
._ch_slider {
  -webkit-appearance: none;
  width: 100%;
  height: 10px;
  border-radius: 5px;
  background: linear-gradient(to right, #f64a6d,#ca399a);
  outline: none;
  opacity: 0.7;
  -webkit-transition: .2s;
  transition: opacity .2s;
}

._ch_slider:hover {
  opacity: 1;
}

._ch_slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #4CAF50;
  cursor: pointer;
}

._ch_slider::-moz-range-thumb {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #4CAF50;
  cursor: pointer;
}

</style>