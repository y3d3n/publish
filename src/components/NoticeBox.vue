<template>
    <div>
        <div v-if="message">
            <div class="_ch_notice_overlay"></div>
            <div class="_ch_notice">
                <div class="_ch_notice_card">
                    <div class="_ch_notife_head">
                        <button class="_ch_faq_head_x" @click="disableNotice">
                            <svg data-v-bbe825aa="" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x"><line data-v-bbe825aa="" x1="18" y1="6" x2="6" y2="18"></line> <line data-v-bbe825aa="" x1="6" y1="6" x2="18" y2="18"></line></svg>
                        </button>                        
                        <h2 class="_ch_faq_head_1">Something's Not Right</h2>
                    </div>
                    <div class="_ch_notife_body">
                        <div class="_ch_faq_card">
                            <div class="_ch_ans">
                                {{ message }}
                            </div>
                        </div>
                    </div>
                </div>            
            </div>
        </div>              
    </div>
</template>
<script>
export default {
    data(){
        return{
            message:""
        }
    },
    methods:{
        disableNotice(){
            this.message = "";
        }
    }
}    
</script>
<style>
    ._ch_notife_head{
        display: flex;      
        margin-bottom: 25px;
        justify-content: center;
    }
    ._ch_faq_head_1{
        font-size: 21px;
        padding: 0;
        margin: 0;
    }
    ._ch_faq_head_x{
        height: 34px;
        width: 34px;
        margin-right: 25px;
        display: flex;
        justify-content: center;
        align-items: center;
        background: none;
        border: 0;
        border-radius: 50%;
        cursor: pointer;
        position: absolute;
        left: 15px;
        top: 15px;
    }
    ._ch_faq_head_x:hover{
        background: rgb(231, 231, 231);
    }
    ._ch_notife_body{
        background: white;

    }
    ._ch_notice{
        z-index: 100;
        position: fixed;
        width: 100%;
        height:100%;
    }
    ._ch_notice_overlay{
        backdrop-filter: blur(8px);
        background: rgba(0,0,0,.6);
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        z-index: 10;
    }
    ._ch_notice_card{
        background: white;
        left: 50%;
        position: absolute;
        top: 50%;
        transform: translate(-50%,-50%);
        border-radius: 15px;
        padding: 15px;
        box-sizing: border-box;
    }
    ._ch_notice_close{
        background: white;
        left: 50%;
        position: absolute;
        top: 50%;
        transform: translate(-50%,-50%);
        border-radius: 15px;
        padding: 15px;
        box-sizing: border-box;
    }
</style>