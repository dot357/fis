<template>
  <section >
    
    <div class="content">
      <div class="left">
        <Builder
          @get-info="getInfo"
          :asText="builderSettings.showAsText"
          :equalTax="builderSettings.equalTax"
          :darkMode="builderSettings.darkMode"
        />
      </div>
      <div class="right"  v-if="show.utilBar">
        <!-- <div class="logInBox">
          <h2>Create an account</h2>
          <p>Here is some benefits for a free account!</p>
          <ul>
            <li><span>🍕</span> Ad Free</li>
            <li><span>✨</span> Dashboard</li>
            <li><span>🤟🏻</span>Remove branding</li>
            <li><span>📱</span>Mobile app</li>
          </ul>
          <div class="buttons">
            <button>Sign Up</button>
            <button>Log in</button>
          </div>
        </div> -->
        <div class="settings">
          <details open>
            <summary>⚙️ Settings</summary>
            <div class="container">
              <p>
                🧮 Show aproximated sum
                <input
                  type="checkbox"
                  name=""
                  id=""
                  v-model="builderSettings.showAsText"
                />
              </p>
              <p class="darkMode">
                🌑 Dark mode
                <input
                  type="checkbox"
                  name=""
                  id=""
                  v-model="builderSettings.darkMode"
                  @change="darkMode"
                />
              </p>
              <p>
                <button class="generate-pdf" @click="generatePDF(builder)">
                  <span  v-if="!loading.generatePDF">Generate PDF</span
                  ><span v-if="loading.generatePDF">Processing...</span>
                </button>
              </p>
            </div>
          </details>
        </div>
      </div>
    </div>
    <div class="mobileMenu" @click="showMenu">
     <div class="icon">
      <div class="bar"></div>
      <div class="bar"></div>
      <div class="bar"></div>
     </div>
    </div>
  </section>
</template>

<script>
import Builder from '../components/Builder.vue'

import pdfMake from '../node_modules/pdfmake'
export default {
  components: { Builder },
  name: 'BuilderPage',
  data() {
    return {
      show : {
        utilBar : false
      },
      loading: {
        generatePDF: false,
      },
      builderSettings: {
        showAsText: true,
        darkMode: false,
      },
      builder: {},
    }
  },
  methods: {
    showMenu(){
      this.show.utilBar = !this.show.utilBar
      
    },
    getInfo($e) {
      this.builder = $e
      console.log($e)
    },
    darkMode() {
      if (this.builderSettings.darkMode) {
        document.querySelector('body').style.filter = 'invert(1)'
        !this.builderSettings.darkMode
      } else {
        document.querySelector('body').style.filter = 'invert(0)'
        !this.builderSettings.darkMode
      }
    },
    generatePDF(item) {
      !this.loading.generatePDF
      /* 
      TODO : Add tax rate if universal tax is not selected and display tax rates in the table individually
      */
      let itemTable = [['Item Description', 'Quantity', 'Rate', 'Amount']]

      item.items.forEach((e) => {
        if (item.info.universalTax)
          itemTable.push([
            { text: e.desc, bold: true },
            e.quantity,
            e.rate,
            e.quantity * e.rate,
          ])
        else
          itemTable.push([
            { text: e.desc, bold: true },
            e.quantity,
            e.rate,
            `${e.quantity * e.rate} ( ${e.tax}%)`,
          ])
      })

      let toBePrinted = {
        content: [
          {
            alignment: 'left',
            columns: [
              {
                width: 160,
                text: [
                  {
                    text: `${item.header.yourCompany.cName} \n `,
                    fontSize: 14,
                    bold: true,
                    lineHeight: 1.25,
                  },
                  {
                    text: `${item.header.yourCompany.cAdress} \n ${item.header.yourCompany.cCity} \n ${item.header.yourCompany.cCountry} \n\n\n`,
                    fontSize: 9.5,
                    bold: false,
                    lineHeight: 1.35,
                  },
                  {
                    text: `${item.header.theirCompany.tName} \n `,
                    fontSize: 14,
                    bold: true,
                    lineHeight: 1.25,
                  },
                  {
                    text: `${item.header.theirCompany.tAdress} \n ${item.header.theirCompany.tCity} \n ${item.header.theirCompany.tCountry}`,
                    fontSize: 9.5,
                    bold: false,
                    lineHeight: 1.35,
                  },
                ],
              },
              {
                width: '*',
                text: [
                  {
                    text: `${item.header.yourCompany.cName} \n\n\n\n\n\n`,
                    alignment: 'right',
                    fontSize: 18,
                    bold: true,
                  },
                  {
                    text: `Invoice# : ${item.info.invoiceNumber} \n Invoice Date : ${item.info.invoiceDate} \n Due Date : ${item.info.dueDate} \n  `,
                    alignment: 'right',
                    fontSize: 9.5,
                    bold: false,
                    lineHeight: 1.35,
                  },
                ],
              },
            ],
            columnGap: 25,
            margin: [0, 0, 0, 50],
          },
          {
            layout: 'lightHorizontalLines', // optional
            table: {
              // headers are automatically repeated if the table spans over multiple pages
              // you can declare how many rows should be treated as headers
              headerRows: 1,
              widths: ['*', 'auto', 100, '*'],

              body: [
                ['First', 'Second', 'Third', 'The last one'],
                ['Value 1', 'Value 2', 'Value 3', 'Value 4'],
                [{ text: 'Bold value', bold: true }, 'Val 2', 'Val 3', 'Val 4'],
              ],
            },
          },
          {
            alignment: 'left',
            columns: [
              {
                width: '*',
                text: [
                  { text: ` `, fontSize: 14, bold: true, lineHeight: 1.25 },
                ],
              },
              {
                width: 185,
                text: [
                  {
                    text: `Sub Total : ${item.calculations.subTotal} ${
                      item.info.currency
                    } \n Taxed (${item.info.tax}%) : ${
                      item.calculations.taxXed
                    } ${item.info.currency}  \n TOTAL : ${JSON.stringify(
                      item.calculations.afterTax
                    ).toUpperCase()} ${item.info.currency} \n  `,
                    alignment: 'justify',
                    fontSize: 11,
                    bold: true,
                    lineHeight: 1.35,
                  },
                  { text: `` },
                ],
              },
            ],
            columnGap: 25,
            margin: [0, 50, 0, 50],
          },
          {
            text: `Notes: \n ${item.footer.notes} \n\n Terms & Conditions : \n ${item.footer.terms}`,
           
          },
        ],
      }
      pdfMake.fonts = {
        Roboto: {
          normal:
            'https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.66/fonts/Roboto/Roboto-Regular.ttf',
          bold: 'https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.66/fonts/Roboto/Roboto-Medium.ttf',
          italics:
            'https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.66/fonts/Roboto/Roboto-Italic.ttf',
          bolditalics:
            'https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.66/fonts/Roboto/Roboto-MediumItalic.ttf',
        },
      }
      toBePrinted.content[1].table.body = itemTable
      toBePrinted.content[2].columns[1].text[1].text = `${item.info.sumAsText} ${item.info.currency}`

      pdfMake.createPdf(toBePrinted).download()
      !this.loading.generatePDF
    },
  },
  watch : {
    'show.utilBar'(newValue){
      newValue ? this.$nextTick(() => {document.querySelector("body").style.overflow = "hidden"}) : this.$nextTick(() => {document.querySelector("body").style.overflow = "scroll"})
    }
  }
}
</script>


<style>
section .content {
  display: flex;
  flex-direction: row;
  gap: 25px;
  padding: 60px;
}
section .content div {
  width: 100%;
}

section .content > .right {
  min-width: 25%;
  width: auto;
  background: var(--base-color);
  border-radius: 4px;
  padding: 15px;
}

section .content .right .settings {
  position: sticky;
  top: 60px;
  background: white;
  border-radius: 4px;
}

section .content .right .logInBox {
  color: white;
  margin-bottom: 25px;
}

section .content .right .logInBox ul {
  list-style: none;
}
section .content .right .logInBox ul li {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 7px;
  margin: 13px 0px;
}

section .content .right .logInBox ul li span {
  display: block;
  width: 25px;
  height: 25px;
  background: white;
  border-radius: 999px;
  text-align: center;
}

section .content .right .logInBox .buttons {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 25px;
}
section .content .right .logInBox .buttons button {
  width: auto;
  padding: 6px 12px;
  outline: none;
  border: none;
}

section .content .right .settings {
  cursor: pointer;
  box-shadow: 0px 0px 25px -7px rgba(0, 0, 0, 0.164);
}

section .content .right .settings details {
  padding: 1rem;
}

section .content .right .settings .container p {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.darkMode {
  background: white;
  padding: 0.3rem 0;
  filter: invert(1);
  border-radius: 4px;
}


.mobileMenu{
  /* get rid of it when desktop */
  width: 55px;
  height: 55px;
  background: var(--base-color);
  position: fixed;
  bottom: 15px;
  right: 15px;
  border-radius: 4px;
  border: 2px solid white;
  cursor: pointer;
}

.mobileMenu .icon{
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 7px;
}

.mobileMenu .icon .bar{
  width: 80%;
  height: 4px;
  background: white;
  border-radius: 999px;
}

.generate-pdf{
  width: 100%;
  padding:12px 12px;
  background: #fc9403;
  border: none;
  outline: none;
  border-radius: 4px;
  box-shadow: 0px 0px 25px 0px #4b2c0038;
  font-weight: bold;
  text-transform: uppercase;
  font-size: 1.1rem;
  margin-top: 25px;
  cursor: pointer;
}

.generate-pdf:hover{
  transition: all 0.15s ease-in-out;
  opacity: 0.8;
}

.generate-pdf:active{
  transform: scale(0.95);
}


@media only screen and (max-width: 760px) {
section .content{
    padding: 0px;
}
form > .header{
  flex-direction: column;
}

form > .header > .right{
  min-height: 250px;
}

form{
  padding: 15px;
}

.table .head, .table .body .single{
  padding: 15px 0;
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 17px;
}
.table .head, .table .body .single div{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0;

  
}

.table .head div:first-child{
  padding: 5px;
}

.table .sum{
  flex-direction: column;
}

.body .single div textarea{
  padding: 0 5px;
}
.body .single div input:first-of-type{
padding-left: 25px;
}


section .content > .right{
  position: fixed;
  height: 100%;
  top: 0;
  left: 0;
  width: 100vw;
  padding: 0 3rem;
  box-sizing: border-box;
  z-index: 0;
   backdrop-filter: blur(16px) saturate(180%);
    -webkit-backdrop-filter: blur(16px) saturate(180%);
    background-color: rgba(17, 25, 40, 0.75);
}



section .content .right .settings{
  position: relative;
}

}
</style>


