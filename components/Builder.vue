<template>
  <section>
    <form>
      <div class="header">
        <div class="left">
        

          <div class="yours">
            <h2>Your Company :</h2>
            <input type="text" :placeholder="fis.test" v-model="fis.test">
            <input type="text" placeholder="Company’s Adress"> 
            <input type="text" placeholder="City, State Zip"> 
            <input type="text" placeholder="Country"> 
          </div>

           <div class="theirs">
             <h2>Bill to :</h2>
            <input type="text" placeholder="Your Client’s Company">
            <input type="text" placeholder="Client’s Adress"> 
            <input type="text" placeholder="City, State Zip"> 
            <input type="text" placeholder="Country"> 
          </div>
        </div>
        <div class="right">

          <input class="getLost" type="file" ref="logo"> <!-- this will be hidden --> 
          <h1 v-html="fis.test"  @click="$refs.logo.click()">INVOICE</h1>

          <div class="info">
           <div class="container">
             <p >Invoice# :</p>
              <input type="text" placeholder="EG : INV-13">
           </div>

           <div class="container">
             <p>Invoice Date :</p>
              <input type="date" >
           </div>

           <div class="container">
             <p>Due Date :</p>
              <input type="date" >
           </div>
            
          </div>
          
        </div>
      </div>
      <div class="table">
        <h2>Table</h2>
      
      
      </div>
      <div class="notes">
        <h2>Notes</h2>
      </div>
    </form>
  </section>
</template>

<script>
export default {
  name: 'Builder',
  data() {
    return {
      fis: {
        test :'Your company'
      },
    }
  },
  methods: {},
}
</script>

<style>
form {
  min-height: 100vh;
  background: white;
  width: 100%;
  border-radius: 4px;
  box-shadow: 0px 0px 25px rgba(0, 0, 0, 0.25);
  padding: 60px;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: flex-start;
}

form  input{
  font-family: 'Josefin Sans', sans-serif;
  color: black;
  border: none;
  outline: none;
  padding: 7px 0;
  font-size: 1.125rem;
  position: relative;
}

form  input::placeholder{
  color: black;
  
  
  
}

.getLost{
  position: absolute;
  z-index: -999;
  opacity: 0;
  pointer-events: none;
}



form  input:focus, form  input:hover{
  font-weight: bold;
}

form div {
  width: 100%;
 
  height: auto;
}

form > .header{
  display: flex;
  flex-direction: row;
  gap: 25px;
  
  align-items: flex-start;
 
}

form > .header > .right{
  width:100%;
 
  min-height: 480px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  
}



form > .header > .right > h1{
  font-size: 3rem;
  text-align: right;
  cursor: pointer;
  padding: 0;
  margin: 0;
}


form > .header > .right .info .container{
  display: flex;
  flex-direction: row;
  width: 100%;
 
  justify-content: space-between;
}

form > .header > .right .info .container{
  border-bottom: 1px solid black;
}

form > .header > .right .info .container p{
  font-weight: bold;
}

form > .header > .right .info .container input{
  
  min-width: 250px;
}


form > .header > .left > div{
  display: flex;
  flex-direction: column;
  gap: 5px;
  
  
}

form > .header .left > .theirs{
  margin-top: 55px;
}




</style>
