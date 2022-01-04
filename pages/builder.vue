<template>
 <section>
  
   <div class="content">
       <div class="left"> <Builder /></div>
       <div class="right">Ads section</div>
   </div>
 </section>
</template>

<script>
import Builder from '../components/Builder.vue'
export default {
  components: { Builder },
  name: 'BuilderPage',
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
    width: 25%;
    background: green;
    border-radius: 4px;
    
}
</style>


