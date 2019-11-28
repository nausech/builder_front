<template>
    <va-card class="card-tab">
        <div class="grid row ma-3">
            <div class="flex md3 sm12">
                <va-button flat small color="gray" icon="fa fa-arrow-left">Atras</va-button>
            </div>
            <!--//Tabs-->
            <div class="flex md9 sm12 align-right tabContainer">
                <div class="btnContainer">
                    <button  v-bind:class="{ actives: meth }" @click="component='forms',meth=true,bas=false,tel=false,edit=false">Metodologia</button>
                    <!-- <button  v-bind:class="{ actives: bas }" @click="component='baseCampaign',bas=true,meth=false,tel=false,edit=false">Base</button>
                    <button  v-bind:class="{ actives: tel }" @click="component='telemarketer',tel=true,meth=false,edit=false,bas=false">Telemercaderista</button>
                    <button  v-bind:class="{ actives: edit }" @click="component='edition',edit=true,meth=false,bas=false,tel=false">Modificadores</button>
                --></div>
            </div>
            <!--//Components Methodology-Base-Telemarketer-->
            <component :is="component"></component>
        </div>
    </va-card>
</template>

<script>
//import methodology from '@/components/forms/Methodology.vue'
//import telemarketer from '@/components/forms/Telemarketer.vue'
//import baseCampaign from '@/components/forms/BaseCampaign.vue'
import form from '@/components/forms/form-elements/FormElements.vue'

export default {
    components: { 
       //methodology, telemarketer, baseCampaign, edition
       form
    },
    props:{
      campanna:{
        type:Number,
        required:true
      }
    },
    data() {
        return {
            component: 'form',
            meth: true,
            bas:false,
            tel:false,
            edit:false
        }
    },
    created(){
      this.cambiarDatos()
    },
    methods: {
      cambiarDatos(id){
        if(this.campanna){
            sessionStorage.setItem('_campanna_id',this.campanna)
        }else{
            console.log("no hay datos")
        }
          
      }
    }
}
</script>

<style>
  .fa-trash, .fa-pencil, .fa-eye, .fa-arrow-left{
    font-size: 20px;
  }
  .align-right{
    text-align: right;
  }
  .tabContainer .btnContainer button{
    width: 25%;
    height: 100%;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 10px;
    background-color: rgb(245, 248, 249);
    border-color: rgb(186, 191, 194);
  }
  .tabContainer .btnContainer button:hover{
    /*background-color: #d7d4d4;*/
    background-image: linear-gradient(to right, rgb(52, 181, 229), rgb(46, 132, 224)); 
    color: rgb(255, 255, 255); 
    box-shadow: rgba(44, 130, 224, 0.4) 0px 0.125rem 0.19rem 0px;
  }
  .tabContainer .btnContainer button.actives{
      /*background-color: #d7d4d4;*/
    background-image: linear-gradient(to right, rgb(52, 181, 229), rgb(46, 132, 224)); 
    color: rgb(255, 255, 255); 
    box-shadow: rgba(44, 130, 224, 0.4) 0px 0.125rem 0.19rem 0px;
  }


    
</style>
