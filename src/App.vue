<template>
  <v-app id="main">
    <header>
      <div id="app">
        <svg x="0px" y="0px" width="auto" height="60px" xml:space="preserve">
          <switch>
            <g i:extraneous="self">
              <text transform="matrix(1 0 0 1 1 54.8394)" fill="#111239" font-family="'MyriadPro-Semibold'" font-size="72">Amor</text>
              <text transform="matrix(1 0 0 1 188.29 22.1519)" fill="#111239" font-family="'MyriadPro-Regular'" font-size="21">em</text>
              <g>
                <text transform="matrix(1 0 0 1 188.29 54.8394)" fill="#111239" font-family="'MyriadPro-Semibold'" font-size="36">Cestas</text>
              </g>
              <line fill="none" stroke="#111239" stroke-miterlimit="10" x1="180.7" y1="6.2" x2="180.7" y2="55.8"/>
            </g>
          </switch>
        </svg>
      </div>
    </header>

    <div id="header"></div>

    <Slider id="slider" :imagesSlider="imagesSlider"/>

    <svg id="wave01" x="0px" y="0px" width="1283px" height="92.9px" viewBox="0 0 1283 92.9" enable-background="new 0 0 1283 92.9" xml:space="preserve">
      <switch>
        <g i:extraneous="self">
          <path fill="#818891" d="M-2.1,53.1c54.3,40.1,126.2,31.1,196.3,4.8C298.7,18.6,458,29.9,549.3,53.5c93,24,255.5,27.1,343.5-5.4
      C1029-2.2,1231,8,1286.1,48.6l0,46.3L-0.3,94"/>
        </g>
      </switch>
    </svg>

    <svg id="wave01" x="0px" y="0px" width="1283px" height="92.9px" viewBox="0 0 1283 92.9" enable-background="new 0 0 1283 92.9" xml:space="preserve">
      <switch>
        <g i:extraneous="self">
          <path fill="#293141" d="M-0.3,62.6c0,0,40.6-17.6,109.6-20.4c153.5-6,247.8,53.4,397.1,33.2s155.4-95.5,517.2-56.5
            c236.4,25.4,260.1,36.7,260.1,36.7v37.4l-1283.9,0L-0.3,62.6z"/>
        </g>
      </switch>
    </svg>

    <div class="contentBasket">
      <v-container class="my-5">
        <v-layout row wrap justify-center>
          <accordion>
            <accordion-item :id="1" :background-color="'rgba(200,200,200,.1)'">
              <template slot="accordion-trigger">
                <h3 class="h3White">Tipo de cesta</h3>
              </template>
              <template slot="accordion-content">
                <span v-for="(item, i) of jsonBasic['basketType']" :key="jsonBasic['basketType'][i]['nome']" :value="i">
                  <Card class="cardProduct" @select="selectBasket(item, i)" @modal="openModal(0, item)" :name="jsonBasic['basketType'][i]['nome']" :description="jsonBasic['basketType'][i]['descricao']" :id="i" :img="jsonBasic['basketType'][i]['imagens'][0]" :selected="basket.id"/>
                </span>
              </template>
            </accordion-item>

            <accordion-item :id="2" :background-color="'rgba(200,200,200,.1)'">
              <template slot="accordion-trigger">
                <h3 class="h3White">Tom de cor dos detalhes</h3>
              </template>
              <template slot="accordion-content">
                <span v-for="(item, i) of jsonBasic['colors']" :key="i" :value="i">
                  <CardColor class="cardProduct" @select="selectColor(item, i)" :id="i" :color1="jsonBasic['colors'][i][0]" :color2="jsonBasic['colors'][i][1]" :selected="color.id"/>
                </span>
              </template>
            </accordion-item>

            <accordion-item :id="2" :background-color="'rgba(200,200,200,.1)'">
              <template slot="accordion-trigger">
                <h3 class="h3White">Quantidade de itens</h3>
              </template>
              <template slot="accordion-content">
                <span v-for="(item, i) of jsonBasic['amountItens']" :key="i" :value="i">
                  <CardPrice v-if="basket.item['nome'] != 'Cesta em Papel (Kraft)'" class="cardProduct" @select="selectAmountItens(item, i, jsonBasic['amountItens'][i]['valor2'])" @modal="openModal(1, item)" :id="i" :qtdItens="jsonBasic['amountItens'][i]['quantidade']" :price="jsonBasic['amountItens'][i]['valor2']" :selected="amountItens.id"/>
                  <CardPrice v-if="basket.item['nome'] == 'Cesta em Papel (Kraft)'" class="cardProduct" @select="selectAmountItens(item, i, jsonBasic['amountItens'][i]['valor'])" @modal="openModal(1, item)" :id="i" :qtdItens="jsonBasic['amountItens'][i]['quantidade']" :price="jsonBasic['amountItens'][i]['valor']" :selected="amountItens.id"/>
                </span>
              </template>
            </accordion-item>
          </accordion>
        </v-layout>
      </v-container>
    </div>

    <v-container class="my-5">
      <v-layout row wrap justify-center>
        <accordion>
          <accordion-item v-for="(group, i) of jsonGroups" :key="i" :value="i" :id="i" :background-color="'rgba(60,60,60,.1)'">
            <template slot="accordion-trigger">
              <h3 class="h3Blue">Grupo {{i}} 
                <span class="labelQtdItens" :style="{'background-color': statusProduct[i-1]}" v-if="amountItens.item">
                  Máximo {{amountItens.item['grupos'][i]}}
                  <span v-if="amountItens.item['grupos'][i] <= 1"> item</span>
                  <span v-if="amountItens.item['grupos'][i] > 1"> itens</span>
                </span>
              </h3>
            </template>
            <template slot="accordion-content">
              <span v-for="(item, j) of jsonGroups[i]" :key="j" :value="j">
                <CardGroup class="cardProduct" @select="selectProduct(item, i-1, j)" @modal="openModal(2, item)" @remove="removeProduct(item, i-1, j)" :name="jsonGroups[i][j]['nome']" :description="jsonGroups[i][j]['descricao']" :id="j" :img="jsonGroups[i][j]['imagens'][0]" :selecteds="groupsItens[i-1]" :shadows="statusProduct[i-1]" :amount="groupsItens[i-1]"/>
              </span>
            </template>
          </accordion-item>
        </accordion>
      </v-layout>
    </v-container>

    <div id="contentWave2">
        <svg id="wave3" x="0px" y="0px" width="1283px" height="92.9px" viewBox="0 0 1283 92.9" enable-background="new 0 0 1283 92.9" xml:space="preserve">
          <switch>
            <g i:extraneous="self">
              <path fill="#818891" d="M1286.1,53.1c-54.3,40.1-126.2,31.1-196.3,4.8C985.2,18.6,826,29.9,734.7,53.5
                c-93,24-255.5,27.1-343.5-5.4C254.9-2.2,52.9,8-2.1,48.6l0,46.3L1284.2,94"/>
            </g>
          </switch>
        </svg>
        <svg id="wave2" x="0px" y="0px" width="1283px" height="92.9px" viewBox="0 0 1283 92.9" enable-background="new 0 0 1283 92.9" xml:space="preserve">
          <switch>
            <g i:extraneous="self">
              <path fill="#293141" d="M-0.3,62.6c0,0,40.6-17.6,109.6-20.4c153.5-6,247.8,53.4,397.1,33.2s155.4-95.5,517.2-56.5
                c236.4,25.4,260.1,36.7,260.1,36.7v37.4l-1283.9,0L-0.3,62.6z"/>
            </g>
          </switch>
        </svg>
    </div>
   
    <div class="contentBasket">
      <v-container class="my-5">
        <v-layout row wrap justify-center>
          <div id="contentContact">
            <h3 class="h3White">Informações para contato</h3>
            <input class="inputNewCategory" type="text" v-model="name" placeholder="Nome"/>
            <input class="inputNewCategory" type="text" v-model="phone" placeholder="Telefone"/>
            <input class="inputNewCategory" type="text" v-model="email" placeholder="Email"/>
            <select id="select" v-model="city" @change="updateAmount()">
              <option v-for="(city, i) of jsonUtils['shipping']" :key="i" :value="i">
                {{jsonUtils['shipping'][i]['nome']}}
              </option>
            </select>
            <select id="select" v-model="payment">
              <option v-for="(payment, i) of jsonUtils['payment']" :key="i" :value="jsonUtils['payment'][i]">
                {{jsonUtils['payment'][i]}}
              </option>
            </select>
            <textarea class="inputNewCategoryObservations" :multiline="true" type="text" v-model="observations" placeholder="Observações"/><br>
          </div>

          <div id="contentContact">
            <h3 class="h3White">Detalhes do pedido</h3>
            <table>
              <tr class="trApp">
                <td>1</td>
                <td>{{basket.item.nome}}</td>
                <td>R$ {{amountItens.item.valor}}</td>
              </tr>
              <tr class="trApp">
                <td></td>
                <td>Frete</td>
                <td>R$ {{jsonUtils['shipping'][this.city]['valor']}},00</td>
              </tr>
            </table>
            <table v-for="(group, i) of groupsItens" :key="i" :value="groupsItens[i]">
                <p class="pTable">Grupo {{i+1}}</p>
                <tr v-for="(item, j) of groupsItens[i]['itens']" :key="j" :value="groupsItens[i]['itens']" class="trApp">
                  <td>{{groupsItens[i]['itens'][j]['quantidade']}}</td>
                  <td>{{groupsItens[i]['itens'][j]['item']['nome']}}</td>
                  <td></td>
                </tr>
            </table>
            <table>
              <tr class="trApp">
                <td><h3 class="h3White">Total</h3></td>
                <td></td>
                <td><h3 class="h3White">R$ {{amount.toString().replace('.', ',')}}</h3></td>
              </tr>
            </table>
          </div>

          <div id="contentContact">
            <p id="alertCity" v-if="jsonUtils['shipping'][city]['nome'] == 'Rodoviária do Plano Piloto*'">
              A entrega na Rodoviária do Plano Piloto poderá ser realizada apenas em dias úteis, entre os horários de 12:00hrs às 14:00hrs.
            </p>
            <button id="buttonSend" @click="send()" :disabled="sendingData">
              <img class="imgLoadSend" v-if="sendingData" src="@/assets/util/load.gif" />
              <span v-if="!sendingData">Finalizar pedido</span>
            </button>
          </div>
        </v-layout>
      </v-container>
    </div>

    <div id="contentWave3">
        <svg id="wave3" x="0px" y="0px" width="1283px" height="92.9px" viewBox="0 0 1283 92.9" enable-background="new 0 0 1283 92.9" xml:space="preserve">
          <switch>
            <g i:extraneous="self">
              <path fill="#818891" d="M-2.1,53.1c54.3,40.1,126.2,31.1,196.3,4.8C298.7,18.6,458,29.9,549.3,53.5c93,24,255.5,27.1,343.5-5.4
      C1029-2.2,1231,8,1286.1,48.6l0,46.3L-0.3,94"/>
            </g>
          </switch>
        </svg>
        <svg id="wave2" x="0px" y="0px" width="1283px" height="92.9px" viewBox="0 0 1283 92.9" enable-background="new 0 0 1283 92.9" xml:space="preserve">
          <switch>
            <g i:extraneous="self">
              <path fill="#FFFFFF" d="M1283.7,62.6c0,0-40.6-17.6-109.6-20.4c-153.5-6-247.8,53.4-397.1,33.2S621.1-20.1,259.3,18.8
                C23,44.3-1.3,55.5-1.3,55.5v37.4l1284.4,0L1283.7,62.6z"/>
            </g>
          </switch>
        </svg>
    </div>

    <footer>
      <a class="aFooter" :href="'http://api.whatsapp.com/send?1=pt_BR&phone=' +  + this.jsonUtils['contact']['telefone']" target="_blank">
        <i class="fab fa-whatsapp"></i>
        WhatsApp
      </a><br>
      <a v-if="jsonUtils['contact']['instagram']" class="aFooter" :href="this.jsonUtils['contact']['instagram']" target="_blank">
        <i class="fab fa-instagram"></i>
        {{jsonUtils['contact']['Usuarioinstagram']}}
      </a><br>
      <a class="aFooter" :href="'tel:' + jsonUtils['contact']['telefone']">
        <i class="fas fa-phone"></i>
        (61) 98493-4430
      </a><br>
      <a class="aFooter" :href="'mailto:' + jsonUtils['contact']['email'] + '?subject=Cliente%20cestas'" target="_blank">
        <i class="far fa-envelope"></i>
        {{jsonUtils['contact']['email']}}
      </a>
    </footer>

    <div id="modals">
      <ModalInfoProduct ref="ModalInfoProduct" />
      <ModalSend ref="ModalSend" @send="sendMail()"/>
    </div>
    <div class="contentToast">
      <transition class="toast" name="toast">
        <Toast v-if="showToast" :color="colorToast" :text="textToast" />
      </transition>
    </div>
  </v-app>
</template>

<script>
import axios from "axios";

import Accordion from "./components/accordion";
import AccordionItem from "./components/accordion-item";
import Card from "./components/Card";
import CardGroup from "./components/CardGroup";
import CardPrice from "./components/CardPrice";
import CardColor from "./components/CardColor";
import Slider from "./components/Slider";
import ModalInfoProduct from "./components/ModalInfoProduct";
import ModalSend from "./components/ModalSend";
import Toast from './components/Toast';

import imptSlider from "./assets/data/slider.json";
import imptBasic from "./assets/data/basic.json";
import imptGroups from "./assets/data/groups.json";
import imptUtil from "./assets/data/util.json";

export default {
  name: "App",
  components: {
    Accordion,
    AccordionItem,

    Card,
    CardGroup,
    CardPrice,
    CardColor,
    Slider,
    ModalInfoProduct,
    ModalSend,
    Toast,
  },
  data() {
    return {
      showToast: false,
      products: [],
      imagesSlider: imptSlider,
      jsonBasic: JSON.parse(JSON.stringify(imptBasic)),
      jsonGroups: JSON.parse(JSON.stringify(imptGroups)),
      jsonUtils: JSON.parse(JSON.stringify(imptUtil)),

      name: '',
      phone: '',
      email: '',
      city: 0,
      payment: 'PIX',
      amount: 0,
      observations: '',

      basket: {id: '', item: ''},
      color: {id: '', item: ''},
      amountItens: {id: '', item: ''},
      groupsItens: [],
      statusProduct: [],

      colorToast: '',
      textToast: '',

      sendingData: false,
    }
  },
  mounted(){
    this.basket.id = 0;
    this.basket.item = this.jsonBasic['basketType'][0];
    this.color.id = 0;
    this.color.item = this.jsonBasic['colors'][0];
    this.amountItens.id = 0;
    this.amountItens.item = this.jsonBasic['amountItens'][0];

    for(var i in this.jsonBasic['amountItens'][0]['grupos']){
      this.groupsItens.push(
        {
          max: this.jsonBasic['amountItens'][0]['grupos'][i],
          itens: []
        }
      );

      this.statusProduct.push('yellow');
    }

    this.updateAmount();
  },
  methods: {
    sendMail(){
      var temp = "<table style='width: 100%;color: #293141;'>";
      for(var i in this.groupsItens){
        var group = parseInt(i) + 1;
        temp = temp + "<p style='color: #707070; border-bottom: 1px solid #707070;'>Grupo " + group + "</p>";
        for(var j in this.groupsItens[i]['itens']){
          temp = temp + "<tr style='width: 100%;border-bottom: 1px solid white;'>";
          temp = temp + "<td style='width: 33%;'>" + this.groupsItens[i]['itens'][j]['quantidade'] + "</td>";
          temp = temp + "<td style='width: 33%;'>" + this.groupsItens[i]['itens'][j]['item']['nome'] + "</td>";
          temp = temp + "<td style='width: 33%;'></td>";
          temp = temp + "</tr>";
        }
      }
      temp = temp + "</table>"

      this.sendingData = true;

      var observations = this.observations;
      if(this.observations == ''){
        observations = "Sem observações adicionais";
      }

      var amount = this.amount;

      var data = {
        "emailAdmin": this.jsonUtils['contact']['email'],
        "email": this.email,
        "message": "<div style='width: 100vw;border-bottom: 1px solid #293141;text-align: left; padding-left: 10px;'>" +
          "<img style='height: 60px; width: auto;' src='https://cestas.epizy.com/logo.png'/>" +
        "</div>" +
        "<div style='color: #293141;'>" +
        "<h3>Dados do comprador: </h3>" +
        "<p>&nbsp;&nbsp;Nome: " + this.name + 
        "</p><p>&nbsp;&nbsp;Telefone: " + this.phone + 
        "</p><p>&nbsp;&nbsp;Email: " + this.email + 
        "</p><p>&nbsp;&nbsp;Cidade: " + this.jsonUtils['shipping'][this.city]['nome'] + 
        "</p><p>&nbsp;&nbsp;Método de pagamento: " + this.payment + 
        "</p><p>&nbsp;&nbsp;Valor: R$ " + amount.toString().replace('.', ',') +
        "</p><p>&nbsp;&nbsp;Observações: " + observations + 
        "</p><h3>Informações da compra: </h3>" +
        "<p>Tom de cor dos detalhes: " + this.color.item[2] + "</p>" +
        "<div style='position: relative; left: 50%; margin-left: -150px; width: 100%; max-width: 300px;'>" +
        "<table style='width: 100%;color: #293141;'>" +
          "<tr style='width: 100%;border-bottom: 1px solid white;'>" +
            "<td style='width: 33%;'>1</td>" +
            "<td style='width: 33%;'>" + this.basket.item.nome + "</td>" +
            "<td style='width: 33%;'>R$ " + this.amountItens.item.valor + "</td>" +
          "</tr>" +
          "<tr style='width: 100%;border-bottom: 1px solid white;'>" +
            "<td style='width: 33%;'></td>" +
            "<td style='width: 33%;'>Frete</td>" +
            "<td style='width: 33%;'>R$ " + this.jsonUtils['shipping'][this.city]['valor'] + ",00</td>" +
          "</tr>" + 
        "</table>" +
        temp +
        "<table style='width: 100%;color: #293141;'>" +
          "<tr style='width: 100%;border-bottom: 1px solid white;'>" +
            "<td style='width: 33%;'><h3>Total</h3></td>" +
            "<td style='width: 33%;'></td>" +
            "<td style='width: 33%;'><h3>R$ " + amount.toString().replace('.', ',') + "</h3></td>" +
          "</tr>" +
        "</table></div></div>"
      };
      var that = this;
      axios.post('https://cestas.epizy.com/mail.php', JSON.stringify(data))
        .then(function (response) {
          if(response.data == 1){
            that.colorToast = "#05e25f";
            that.textToast = "As informações foram enviadas com sucesso, entraremos em contato assim que possível";
            that.showToast = true;
            setTimeout(() => that.showToast = false, 5000);
          }
          that.name = '';
          that.phone = '';
          that.email = '';
          that.observations = '';
          that.sendingData = false;
        })
        .catch(function () {
          that.colorToast = "#ff544c";
          that.textToast = "Ocorreu algum erro inesperado, tente novamente";
          that.dispToastAlert();
          that.sendingData = false;
        });
    },
    dispToastAlert(){
      this.showToast = true;
      setTimeout(() => this.showToast = false, 3000);
    },
    handlerProduct(params){
      if(params){
        this.$refs.ModalInfoProduct.openModal(params);
      }
    },

    updateAmount(){
      this.amount = this.jsonUtils['shipping'][this.city]['valor'] + Number(this.amountItens.item['valor'].replace(',', '.'));
      this.amount = this.amount.toFixed(2);
    },

    send(){
      if(this.name.length == 0){
        this.colorToast = "#ff544c";
        this.textToast = "O nome não foi informado";
        this.dispToastAlert();
        return;
      }
      else if(this.name.length < 5){
        this.colorToast = "#ff544c";
        this.textToast = "O nome informado é muito curto";
        this.dispToastAlert();
        return;
      }
      else if(this.phone.length < 8){
        this.colorToast = "#ff544c";
        this.textToast = "O telefone informado é inválido";
        this.dispToastAlert();
        return;
      }
      else if(this.email.length < 6){
        this.colorToast = "#ff544c";
        this.textToast = "O email informado é inválido";
        this.dispToastAlert();
        return;
      }

      var verify = 0;
      for(var i in this.statusProduct){
        if(this.statusProduct[i] == '#ff544c'){
          verify = 1;
          break;
        }
        else if(this.statusProduct[i] == 'yellow'){
          verify = 2;
        }
      }

      if(verify == 1){
        this.colorToast = "#ff544c";
        this.textToast = "Foram selecionado itens em quantidade superior a permitida";
        this.dispToastAlert();
        return;
      }
      else if(verify == 2){
        this.$refs.ModalSend.openModal();
        return;
      }

      this.sendMail();
    },

    openModal(type, item){
      this.$refs.ModalInfoProduct.openModal(type, item);
    },
    selectBasket(item, id){
      this.basket.id = id;
      this.basket.item = item;
      
      if(this.basket.item['nome'] == 'Cesta em Papel (Kraft)'){
        this.amountItens.item['valor'] = this.amountItens.item['valor1'];
      }
      else {
        this.amountItens.item['valor'] = this.amountItens.item['valor2'];
      }
      this.updateAmount();
    },
    selectColor(item, id){
      this.color.id = id;
      this.color.item = item;
    },
    selectAmountItens(item, id, valor){
      this.amountItens.id = id;
      this.amountItens.item = item;
      this.amountItens.item['valor'] = valor;

      this.groupsItens.length = 0;
      this.statusProduct.length = 0;
      for(var i in item['grupos']){
        this.groupsItens.push(
          {
            max: item['grupos'][i],
            itens: []
          }
        );
        this.statusProduct.push('yellow');
      }
      this.updateAmount();
    },
    selectProduct(item, i, j){
      var notExists = true;
      var count = 0;
      for(var k in this.groupsItens[i]['itens']){
        if(this.groupsItens[i]['itens'][k]['id'] == j){
          notExists= false;
          this.groupsItens[i]['itens'][k]['quantidade'] = this.groupsItens[i]['itens'][k]['quantidade'] + 1;
        }
        count = count + this.groupsItens[i]['itens'][k]['quantidade'];
      }
      if(notExists){
        count = count + 1;
        this.groupsItens[i]['itens'].push(
          {
            id: j,
            item: item,
            quantidade: 1
          }
        );
      }
      if(count == this.amountItens.item['grupos'][i+1]){
        this.statusProduct[i] = '#05e25f';
      }
      else if(count > this.amountItens.item['grupos'][i+1]){
        this.statusProduct[i] = '#ff544c';
      }
      else{
        this.statusProduct[i] = 'yellow';
      }
      this.statusProduct.push('yellow');
      this.statusProduct.pop();
    },
    removeProduct(item, i, j){
      for(var k in this.groupsItens[i]['itens']){
        if(this.groupsItens[i]['itens'][k]['id'] == j){
          if(this.groupsItens[i]['itens'][k]['quantidade'] < 2){
            this.groupsItens[i]['itens'].splice(k, 1);  
          }
          else{
            this.groupsItens[i]['itens'][k]['quantidade'] = this.groupsItens[i]['itens'][k]['quantidade'] - 1;
          }
          
        }
      }
      var count = 0;
      for(var l in this.groupsItens[i]['itens']){
        count = count + this.groupsItens[i]['itens'][l]['quantidade'];
      }
      if(count == this.amountItens.item['grupos'][i+1]){
        this.statusProduct[i] = '#05e25f';
      }
      else if(count > this.amountItens.item['grupos'][i+1]){
        this.statusProduct[i] = '#ff544c';
      }
      else{
        this.statusProduct[i] = 'yellow';
      }
      this.statusProduct.push('yellow');
      this.statusProduct.pop();
    }
  },
}
</script>

<style scoped>
body {
  font-family: 'Source Sans Pro', 'Helvetica Neue', Arial, sans-serif;
  text-rendering: optimizelegibility;
  -moz-osx-font-smoothing: grayscale;
  -moz-text-size-adjust: none;
}
header{
  position: fixed;
  left: 0px;
  top: 0px;
  z-index: 99;
  background-color: white;
}
  #header{
    position: relative;
    left: 0px;
    top: 0px;
    height: 60px;
    width: 100%;
  }
#main {
  background-color: white;
  width: 100vw;
  overflow: hidden;
}
  #app {
    width: 100vw;
    border-bottom: 1px solid #293141;
    padding-left: 10px;
  }

  #slider {
    position: relative;
    top: 0px;
    width: 99%;
  }

  #wave01 {
    position: absolute;
    left: 0px;
    top: calc(80vh - 10px);
  }

  .contentBasket {
    background-color: #293141;
    width: 100vw;
    text-align: center;
  }
    .h3White {
      color: white;
      margin-top: 25px;
      margin-bottom: 25px;
      text-align: center;
      width: 100%;
    }
    .h3Blue {
      color: #293141;
      margin-top: 25px;
      margin-bottom: 25px;
      text-align: center;
      width: 100%;
    }

  #contentGroup {
    width: 100%;
    overflow-x: scroll;
    overflow-y: hidden;
    height: 330px;
    display: inline-block;
    position: relative;
    text-align: center;
  }
    .cardProduct {
      display: inline-block;
      position: relative;
      margin: 5px 15px 5px 15px;
      text-align: center;
    }
    #select {
      width: 300px;
      height: 35px;
      border-radius: 5px;
      border: 1px solid #293141;
      font-size: 15px;
      margin: 10px 2px 2px 2px;
      padding: 4px;
      background-color: white !important;
      color: #293141;
      overflow: hidden;
      cursor:pointer;
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      background: url(data:image/svg+xml;charset=utf-8;base64,PD94bWwgdmVyc2lvbj0iMS4wIj8+PHN2ZyB2ZXJzaW9uPSIxLjEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4IiB3aWR0aD0iMzA2cHgiIGhlaWdodD0iMzA2cHgiIHZpZXdCb3g9IjAgMCAzMDYgMzA2IiBzdHlsZT0iZW5hYmxlLWJhY2tncm91bmQ6bmV3IDAgMCAzMDYgMzA2OyIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+PGc+PHBvbHlnb24gcG9pbnRzPSIyNzAuMyw1OC42NSAxNTMsMTc1Ljk1IDM1LjcsNTguNjUgMCw5NC4zNSAxNTMsMjQ3LjM1IDMwNiw5NC4zNSIvPjwvZz48L3N2Zz4=) no-repeat;
      background-size: 8px 8px;
      background-position: 95% 50%;
    }
      #select:focus {
          box-shadow: 0 0 0 0;
          border: 2px solid black;
          outline: 0;
      }
  .labelQtdItens {
    border-radius: 30px;
    padding: 5px 10px 5px 10px;
    color: white;
    font-size: 16px;
  }

  #contentWave2 {
    position: relative;
    left: 0px;
    margin: 0px 0px -8px 0px;
  }
    #wave2 {
      position: absolute;
      left: 0px;
      top: 0px;
    }
    #wave3 {
      position: relative;
    }
  #contentWave3 {
    background-color: #293141;
    position: relative;
    left: 0px;
    margin: 0px 0px -8px 0px;
  }

  #contentContact {
    width: 50%;
    min-width:  300px;
    text-align: center;
  }
  .inputNewCategory {
    width: 300px;
    height: 35px;
    border-radius: 5px;
    border: 1px solid #293141;
    font-size: 15px;
    margin: 10px 2px 2px 2px;
    padding: 4px;
    background-color: white;
    color: #293141;
  }
    .inputNewCategory:focus {
      border: 2px solid black;
      box-shadow: 0 0 0 0;
      outline: 0;
    }
  .inputNewCategoryObservations {
    width: 300px;
    height: 90px;
    border-radius: 5px;
    border: 1px solid #293141;
    font-size: 15px;
    margin: 10px 2px 2px 2px;
    padding: 4px;
    background-color: white;
    color: #293141;

  }
    
  #modals {
    z-index: 100;
  }

  #buttonSend {
    background-color: #05e25f;
    color: white;
    font-weight: bolder;
    border-radius: 30px;
    padding: 5px 15px 5px 15px;
    margin-top: 20px;
    width: 150px;
    height: 35px;
  }

  .imgLoadSend {
    width: 18px;
    height: 18px;
    margin-top: 3px;
    margin-bottom: 0px;
  }

  table {
    color: white;
    width: 100%;
  }
     tr {
      width: 100%;
      border-bottom: 1px solid white;
     }
     td {
      width: 33%;
     }
     .pTable {
      border-bottom: 1px solid white;
     }

  #alertCity {
    color: white;
    text-align: center;
    background-color: #ff544c;
    border-radius: 70px;
    margin-top: 5px;
    font-weight: bolder;
    padding: 3px 15px 3px 15px;
  }

  footer {
    z-index: 10;
    background-color: white;
    color: #293141;
    text-align: center;
    height: 130px;
    padding-top: 15px;
  }

  .aFooter {
    text-decoration: none;
    color: #293141;
  }

  .contentToast{
    z-index: 1000;
  }

  .toast-enter-active {
    animation: wobble 0.5s ease;
  }
  /* leave transitions */
  .toast-leave-to {
    opacity: 0;
    transform: translateY(-60px);
  }
  .toast-leave-active {
    transition: all 0.3s ease;
  }
  @keyframes wobble {
    0% { transform: translateY(-100px); opacity: 0 }
    50% { transform: translateY(0px); opacity: 1 }
    60% { transform: translateX(8px); opacity: 1 }
    70% { transform: translateX(-8px); opacity: 1 }
    80% { transform: translateX(4px); opacity: 1 }
    90% { transform: translateX(-4px); opacity: 1 }
    100% { transform: translateX(0px); opacity: 1 }
  }

  .toast {
    z-index: 120;
  }
</style>