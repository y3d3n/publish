<template>
    <div height="500px">
        <div>
            <div>
                <span>That’s all we need, unless you’d like to set customization options.</span>

            </div>
            <div style="position:relative">
                <code type="text" class="EmbedCode">
                    {{ customMessage }}
                    </code>
                <button @click.prevent="copyTextToClipboard">Copy</button>
            </div>
        </div>
        <div>
            <div>Preview</div>
            <div v-html="body"></div>
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
       
    },
    computed:{
        customMessage() {
            return `<div class="cubehall-embed-block" data-view="${this.type}" width="${this.width}px" height="${this.height}px"><a href="${this.embed}" target="_blank" data-link>View On CubeHall</a></div>`;
        }
    },

}
</script>

<style>
.EmbedCode {
    white-space: nowrap;
    position: absolute;
    top: 0;
    color: #8899a6;
    padding: 3px 4px 4px 15px;
    overflow: hidden;
    display: block;
    text-overflow: ellipsis;
    box-shadow: inset 1px 1px 2px 0 rgba(0,0,0,.1);
}

</style>