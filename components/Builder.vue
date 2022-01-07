<template>
  <section>
   <details>
     <summary>F.I.S.</summary>
     <div>
        <pre>
      {{fis}}
    </pre>
     </div>
   </details>
    <form>
   
      <div class="header">
        <div class="left">
          <div class="yours">
            <h2>Your Company</h2>
            <input type="text" placeholder="Company's Name" required v-model="fis.header.yourCompany.cName" />
            <input type="text" placeholder="Company’s Adress" required v-model="fis.header.yourCompany.cAdress" />
            <input type="text" placeholder="City, State Zip" required v-model="fis.header.yourCompany.cCity"/>
            <input type="text" placeholder="Country" required v-model="fis.header.yourCompany.cCountry"/>
          </div>

          <div class="theirs">
            <h2>Bill to :</h2>
            <input type="text" placeholder="Your Client’s Company" required  v-model="fis.header.yourCompany.tName"/>
            <input type="text" placeholder="Client’s Adress" required v-model="fis.header.yourCompany.tAdress"/>
            <input type="text" placeholder="City, State Zip" required v-model="fis.header.yourCompany.tCity" />
            <input type="text" placeholder="Country" required  v-model="fis.header.yourCompany.tCountry" />
          </div>
        </div>
        <div class="right">
          <input class="getLost" type="file" ref="logo" />
          <!-- this will be hidden -->
          <h1  @click="$refs.logo.click()">{{fis.header.yourCompany.cName}}</h1>

          <div class="info">
            <div class="container">
              <p>Invoice# :</p>
              <input type="text" placeholder="EG : INV-13" />
            </div>

            <div class="container">
              <p>Invoice Date :</p>
              <input type="date" />
            </div>

            <div class="container">
              <p>Due Date :</p>
              <input type="date" />
            </div>
          </div>
        </div>
      </div>
      <div class="table">
        
        
  
        <div class="head">
          <div>Item Description</div>
          <div>Quantity</div>
          <div>Rate</div>
          <div>Amount</div>
        </div>
        <div class="body">
      
          <div v-for="(item,index) in fis.items" :key="index" class="single" @input="calculate" >
            <div class="removeItem" @click.prevent="removeItem(item.id)">X</div>
            <div><textarea type="text" placeholder="Enter item name/description" v-model="item.desc" required></textarea></div>
            <div><input type="number"  min="1" placeholder="2" v-model="item.quantity" required></div>
            <div><input type="number"  min="1" placeholder="100" v-model="item.rate" required></div>
            <div v-if="item.rate && item.quantity">{{item.rate * item.quantity}}</div>
         
            <div class="taxRate" v-show="!fis.info.universalTax"><input type="number" :placeholder="item.tax+'%'" v-model="item.tax" ></div>
           
          </div>
          

          
          
        </div>

      <div class="sum">
        <div class="left">
          <button @click.prevent="addItem"> Add Item</button>
        </div>
        <div class="right">
          <div>
            <div>
              <p><strong>Sub Total</strong> <span>{{fis.calculations.subTotal}}</span></p>
            </div>
           <div>
              <p v-if="!fis.info.universalTax"><strong>All Tax Applied </strong>  <span>{{fis.calculations.taxXed}}</span> </p>
              <p v-if="fis.info.universalTax"><strong>Sales Tax ( <span >{{fis.info.tax}}%</span> )</strong>  <span>{{fis.calculations.taxXed}}</span> </p>
              <p @input="calculate"><strong>Equal tax:</strong> <input type="number" min="0" v-model="fis.info.tax"> <input type="checkbox"  name="universalTax" v-model="fis.info.universalTax" @change="calculate"></p>
           </div>
           <div class="total">
              <p ><strong>Total</strong></p>
              <input type="text" placeholder="Currency" v-model="fis.info.currency" >
              <p>{{fis.calculations.afterTax}}  <span style="text-transform:uppercase;margin-left:5px;">{{fis.info.currency}}</span></p>
           </div>
           
          </div>
         
        </div>

        

      </div>
        
        

      </div>
      <div class="notes">
       
        <p v-if="fis.info.sumAsText && asText" class="asText">
          <strong>Approximately : </strong> <span>   {{fis.info.sumAsText}} {{fis.info.currency}}</span>
        </p>
        
     
        <div class="container">
          <h2>Notes :</h2>
          <textarea name="" placeholder="Please add your notes"></textarea>
        </div>

        <div class="container">
          <h2>Terms & Conditions :</h2>
          <textarea
            name=""
            placeholder="Please add your terms and conditions"
          ></textarea>
        </div>
      </div>
    </form>
  </section>
</template>

<script>
import writtenNumber from '../node_modules/written-number'

export default {
  name: 'Builder',
  props : {
     asText : {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      fis: {
          
          test: 'F.I.S.',
          info : {
            tax : 18,
            invoiceNumber : undefined,
            invoiceDate : undefined,
            dueDate : undefined,
            currency : 'USD',
            universalTax : true
           
          },
          calculations : {
            subTotal : 0,
            afterTax : 0,
            taxXed : 0,
            sumAsText : undefined
          },
          header : {
             yourCompany : {
                cName : undefined,
                cAdress : undefined,
                cCity : undefined,
                cCountry : undefined,
                cLogo : undefined
              },
              theirCompany : {
                tName : undefined,
                tAdress : undefined,
                tCity : undefined,
                tCountry : undefined
              }
          },
        items : [
          {
          id: 1,
          desc : undefined,
          quantity : undefined,
          rate : undefined,
          amount : undefined,
          tax : 10
        }
        ]
      },
    }
  },
  methods: {
    calculate(){
     
      this.fis.calculations.subTotal = 0
      this.fis.calculations.taxXed = 0
      this.fis.calculations.afterTax = 0
      this.fis.info.sumAsText = undefined
      this.fis.items.forEach((element) => {
        
        if(this.fis.info.universalTax) {
          
          element.tax = this.fis.info.tax

          if(element.rate && element.quantity){
          //validation logic will be presented here.
          // values are added now calculate the total price
          const taxValue = this.fis.info.tax / 100
          this.fis.calculations.subTotal += this.rounder(element.rate * element.quantity)
          this.fis.calculations.taxXed += this.rounder((element.rate * element.quantity) * taxValue)
          this.fis.calculations.afterTax += this.rounder((element.rate * element.quantity) + ((element.rate * element.quantity) * taxValue ))
          this.fis.info.sumAsText = writtenNumber(this.fis.calculations.afterTax , {lang: document.querySelector("html").lang})
        }
        else console.log("Please enter all required fields")

        }else{
          //calc differently
          
          const individualTaxValue = element.tax / 100
          this.fis.calculations.subTotal += this.rounder(element.rate * element.quantity)
          this.fis.calculations.taxXed += this.rounder((element.rate * element.quantity) * individualTaxValue)
          this.fis.calculations.afterTax += this.rounder((element.rate * element.quantity) + ((element.rate * element.quantity) * individualTaxValue ))
          this.fis.info.sumAsText = writtenNumber(this.fis.calculations.afterTax , {lang: document.querySelector("html").lang})
        }
        
        
      })
    },
    rounder(number){
      return Math.round(number * 100) / 100
    },
    addItem(){
      this.fis.items.push({
          id: this.fis.items.length+1,
          desc : undefined,
          quantity : undefined,
          rate : undefined,
          amount : undefined,
          tax : undefined
      })
    },
    removeItem(id){
      this.fis.items = this.fis.items.filter(e => e.id !== id)
      this.calculate()
    }
  },
}
</script>

<style>
.getLost {
  position: absolute;
  z-index: -999;
  opacity: 0;
  pointer-events: none;
}

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

form > div {
  margin: 25px 0;
  /*border: 1px solid rgba(0, 0, 0, 0.178); */
}

form input {
  font-family: 'Josefin Sans', sans-serif;
  color: black;
  border: none;
  outline: none;
  padding: 7px 0;
  font-size: 1.125rem;
  position: relative;
}

form  input::placeholder {
  color: rgba(0, 0, 0, 0.473);
}



form div {
  width: 100%;

  height: auto;
}

form > .header {
  display: flex;
  flex-direction: row;
  gap: 25px;

  align-items: flex-start;
}

form > .header > .right {
  width: 100%;

  min-height: 480px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

form > .header > .right > h1 {
  font-size: 3rem;
  text-align: right;
  cursor: pointer;
  padding: 0;
  margin: 0;
}

form > .header > .right .info .container {
  display: flex;
  flex-direction: row;
  width: 100%;

  justify-content: space-between;
}

form > .header > .right .info .container {
  border-bottom: 1px solid black;
}

form > .header > .right .info .container p {
  font-weight: bold;
}

form > .header > .right .info .container input {
  min-width: 250px;
}

form > .header > .left > div {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

form > .header .left > .theirs {
  margin-top: 55px;
}

form  textarea {
  width: 100%;
  height: 70px;
  resize: none;
  outline: none;
  border: none;
  padding: 7px 0;
  font-size: 1.125rem;
  font-family: 'Josefin Sans', sans-serif;
  position: relative;
}


form .notes .container textarea::placeholder {
  color: rgba(0, 0, 0, 0.473);
}

form .notes .container textarea:focus,
form .notes .container textarea:hover {
  font-weight: bold;
}

.table .head{
  background: var(--base-color);
  color: white;
  padding: 20px 0px;

  
}

.table .head {
  border-radius: 4px;

}

.table .head  div:first-child{

  padding-left: 30px;

}
.body .single{
  padding: 20px 0px;
}

.body .single div:first-child{
   padding-left: 30px;
   width: 100%;
  
}
.table .body .single{
  position: relative;

}
.table .body .single .taxRate{
  position: absolute;
  left: 93.5%;
  top: 10px;
  background: var(--base-color);
  width: 50px;
  padding: 5px;
  border-radius: 4px;
  
  opacity: 0;
  z-index: -1;
  pointer-events: none;

}

.table .body .single:hover .taxRate{
  transition: all 0.15s ease;
  z-index: initial;
  
  
   pointer-events: all;
  opacity: 1;
}

.table .body .single .removeItem{
  position: absolute;
  right:0;
  top: 10px;
  cursor: pointer;
  font-weight: bold;
  color: white;
  height: 30px;
  width: 25px;
 
  background: var(--danger-color);
  width: auto;
  padding: 5px;
  border-radius: 4px;

   opacity: 0;
 
  pointer-events: none;
  display: flex;
  justify-content: center;
  align-content: center;

}



.table .body .single:hover .removeItem{
  transition: all 0.15s ease;
  z-index: initial;
  transition-delay: 0.3s;
  
  
   pointer-events: all;
  opacity: 1;
}

.removeItem:hover{
  transform: scale(0.95);
  transition-delay: 0;
}

.body .single div input{
  width: 50px;
}

.body .single div input::placeholder{
  
  color: rgba(0, 0, 0, 0.548);
}

.body .single div textarea{
  padding: 0 2.6rem;

}

.table .head, .table .body .single{
  display: grid;
grid-template-columns: 4fr repeat(3, 1fr);
}

.table .body .single > div {

  align-items: center;
}

.table .body .single > div > textarea{
  
  max-width: 90%;
  height: 20px;
  
}


.table .sum{
  display: flex;
  flex-direction: row;
   justify-content: space-between;
   align-items: flex-end;
   gap: 25px;
}

.table .sum .right p{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
 .table .sum .right .total{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  background: var(--base-color);
  color: white;
  border-radius: 4px;

}

.table .sum .right .total > *:not(select){
  padding: 8px 20px;
}

.table .sum .right .total input{
  border: none;
  outline: none;
  border-radius: 4px;
  padding: 6px 20px;
  width: 75px;
  text-align: center;
  
}


.asText{
  margin-top: 25px;
  text-transform: capitalize;
  font-weight: 400;
  font-size: 1.3rem;
  text-align: center;
}





.table .sum .left button{
  width: 125px;
  margin: 0 auto;
  border: none;
  outline: none;
  padding: 12px 12px;
  font-weight: bold;
  background: var(--base-color);
  color: white;
  border-radius: 4px;
  cursor: pointer;

}

.table .sum .left button:hover{
  transition: all 0.15s ease;
  transform: scale(0.95);
}



/* media queries */

@media screen and (max-width: 1200px) {
section .content{
  flex-wrap: wrap;
 
}
}


</style>
