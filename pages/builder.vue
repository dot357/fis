<template>
 <section>
  
   <div class="content">
       <div class="left"> <Builder :asText="builderSettings.showAsText" /></div>
       <div class="right">
         <div class="settings">
           <details>
             <summary>⚙️ Settings</summary>
             <div class="container">
               <p>Show aproximated sum <input type="checkbox" name="" id="" v-model="builderSettings.showAsText"></p>
             </div>
           </details>
         </div>
       </div>
   </div>
 </section>
</template>

<script>
import Builder from '../components/Builder.vue'
export default {
  components: { Builder },
  name: 'BuilderPage',
  data(){
    return{
      builderSettings : {
        showAsText : true
      }
    }
  }
}
</script>


<style>

section .content{
    display: flex;
    flex-direction: row;
    gap: 165px;
    padding: 60px;
}
section .content div{
    width: 100%;
}

section .content > .right{
    
    min-width: 25%;
    width: auto;
    background: green;
    border-radius: 4px;
    padding: 15px;
   
    
}

section .content .right .settings{
  position: sticky;
  top: 0;
  background: white;

}
</style>


