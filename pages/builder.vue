<template>
 <section>
  
   <div class="content">
       <div class="left"> <Builder @get-info="getInfo" :asText="builderSettings.showAsText" :equalTax="builderSettings.equalTax" :darkMode="builderSettings.darkMode" /></div>
       <div class="right">
          <div class="logInBox">
             <h2>Create an account</h2>
             <p>Here is some benefits for a free account!</p>
             <ul>
               <li> <span>🍕</span> Ad Free</li>
               <li> <span>✨</span> Dashboard</li>
               <li><span>🤟🏻</span>Remove branding</li>
               <li><span>📱</span>Mobile app</li>
             </ul>
             <div class="buttons">
               <button>Sign Up</button>
               <button>Log in</button>
             </div>
           </div>
         <div class="settings">
          
           <details open>
             <summary>⚙️ Settings</summary>
             <div class="container">
               <p>🧮 Show aproximated sum <input type="checkbox" name="" id="" v-model="builderSettings.showAsText"></p>
               <p class="darkMode">🌑 Dark mode <input type="checkbox" name="" id="" v-model="builderSettings.darkMode" @change="darkMode"></p>
               <p><button @click="generatePDF(builder)">Generate PDF</button></p>
             </div>
           </details>
         </div>
       </div>
   </div>
 </section>
</template>

<script>
import Builder from '../components/Builder.vue'
// documentation for jsPDF https://github.com/parallax/jsPDF http://raw.githack.com/MrRio/jsPDF/master/docs/
import { jsPDF } from "jspdf";
export default {
  components: { Builder },
  name: 'BuilderPage',
  data(){
    return{
      builderSettings : {
        showAsText : true,
        darkMode : false
      },
      builder : {}
    }
  },
  methods : {
    getInfo($e){
      this.builder = $e
      console.log($e)
    },
    darkMode(){
      
      if(this.builderSettings.darkMode){
         document.querySelector('body').style.filter ="invert(1)"
         !this.builderSettings.darkMode
      }
      else {
        document.querySelector('body').style.filter ="invert(0)"
        !this.builderSettings.darkMode
      }


      
    },
    generatePDF(item){
      /* 
      text splitting : http://raw.githack.com/MrRio/jsPDF/master/

      */
      // Default export is a4 paper, portrait, using millimeters for units
      const doc = new jsPDF();

      //you have to do some templating with the pdf to get the result you want.
      //need text splitting will be worked on it

      //Your company
      doc.setFont("helvetica", "bold");  //fonts can be dynamic and users can pick it up later on.
      doc.setFontSize(14);
      doc.text(item.header.yourCompany.cName, 10, 25);
      doc.setFont("helvetica", "normal");
      doc.setFontSize(12);
      doc.text(item.header.yourCompany.cAdress, 10, 32);  
      doc.text(item.header.yourCompany.cCity, 10, 39);    
      doc.text(item.header.yourCompany.cCountry, 10, 46); 
      //their company

      doc.setFont("helvetica", "bold");  //fonts can be dynamic and users can pick it up later on.
      doc.setFontSize(14);
      doc.text(item.header.theirCompany.tName, 10, 60);
      doc.setFont("helvetica", "normal");
      doc.setFontSize(12);
      doc.text(item.header.theirCompany.tAdress, 10, 67);  
      doc.text(item.header.theirCompany.tCity, 10, 74);    
      doc.text(item.header.theirCompany.tCountry, 10, 81); 


   

      doc.save("a4.pdf");
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
    background: var(--base-color);
    border-radius: 4px;
    padding: 15px;
   
    
}

section .content .right .settings{
  position: sticky;
  top: 60px;
  background: white;
  border-radius: 4px;
 

}

section .content .right .logInBox{
  color: white;
  margin-bottom: 25px;
}

section .content .right .logInBox ul{
  list-style: none;
}
section .content .right .logInBox ul li {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 7px;
  margin: 13px 0px;
}

section .content .right .logInBox ul li  span{
  display: block;
  width: 25px;
  height: 25px;
  background: white;
  border-radius: 999px;
  text-align: center;
 
  
}

section .content .right .logInBox .buttons{
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 25px;
}
section .content .right .logInBox .buttons button{
  width: auto;
  padding: 6px 12px;
  outline: none;
  border: none;
}

section .content .right .settings{
  cursor: pointer;
  box-shadow: 0px 0px 25px -7px rgba(0, 0, 0, 0.164);
}

section .content .right .settings details{
  padding: 1rem;
}




section .content .right .settings .container p{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  
}

.darkMode{
  background: white;
  padding: 0.3rem 0;
  filter: invert(1);
  border-radius: 4px;

}
</style>


