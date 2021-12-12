<template>
	<section class="contentSection">
      <div @click="openModal()" class="cardTitle">
        <p class="qtdItens">{{ qtdItens }}<br/>itens</p>
        <div v-if="selected == id" class="itemSelected">
          Selecionado
        </div>
      </div>
      <div class="cardText">
        <p class="pNameProduct">R$ {{price}}</p>
      </div>
      <div class="cardActions">
        <button id="buttonAdd" v-if="selected != id" @click="select()">Selecionar</button>
      </div>
  </section>
</template>

<script>

export default {
  name: "Card",
  components: {
  },
  props: {
    qtdItens: String,
    price: String,
    button: Boolean,
    selected: Number,
    id: Number,
	},
  mounted(){
    this.button = false;
  },
  methods: {
    openModal(){
      this.$emit('modal');
    },
    select(){
      this.$emit('select');
    },
  },
}
</script>

<style scoped>
.contentSection {
  position: relative;
  width: 200px;
  height: 250px;
}
.cardTitle {
  position: relative;
  left: 25px;
  top: 0px;
  width: 150px;
  height: 150px;
  border-radius: 100px;
  background-color:  white;
  overflow: hidden;
}
.cardTitle:hover {
  cursor: pointer;
}
.qtdItens {
  position: absolute;
  width: 150px;
  text-align: center;
  top: 45px;
  left: -5px;
  font-size: 20px;
  font-weight: bolder;
}
.itemSelected {
  width: 100%;
  height: 100%;
  background-color: rgba(50,50,50,.5);
  color: white;
  font-weight: bolder;
  position: absolute;
  top: 0px;
  left: 0px;
  text-align: center;
  padding-top: 65px;
}

.cardText {
  padding-top: 8px;
  color: white;
  text-align: center;
  height: 40px;
  width: 100%;
  position: relative;
  overflow: hidden;
}
  .pNameProduct {
    font-size: 16px;
    margin: 0px;
    font-weight: 400;
    color: white;
    white-space: nowrap;
    width: 100%;
    overflow: hidden;
    padding-top: 5px;
  }
.cardActions {
  position: relative;
  left: 0px;
  width: 100%;
  height: 30px;
}
  #buttonAdd {
    width: 110px;
    height: 30px;
    color: #2c3e50;
    margin: 5px;
    left: 50%;
    margin-left: -55px;
    position: absolute;
    background-color: #05e25f;
    color: white;
    border-radius: 15px;
    font-weight: bolder;
  }
</style>