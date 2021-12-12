<template>
	<section class="contentSection">
      <div @click="openModal()" class="cardTitle" :style="{backgroundImage: 'url('+ require('@/' + img) + ')', 'border': '2px solid ' + getShadows(selecteds)}">
        <div v-if="selected" class="itemSelected">
          Selecionado
        </div>
        <div class="qtd" v-if="getAmountItem(amount)">
          x {{ getAmountItem(amount) }}
        </div>
      </div>
      <div class="cardText">
        <p class="pNameProduct">{{ name }}</p>
      </div>
      <div class="cardActions">
        <button id="buttonAdd" @click="select()">Adicionar</button>
        <button id="buttonRemove" v-if="verify(selecteds)" @click="remove()">Remover</button>
      </div>
  </section>
</template>

<script>

export default {
  name: "Card",
  props: {
    name: String,
    description: String,
    id: Number,
    img: String,
    selecteds: [],
    shadows: String,
    amount: []
	},
  methods: {
    openModal(){
      this.$emit('modal');
    },
    select(){
      this.$emit('select');
    },
    remove(){
      this.$emit('remove');
    },
    verify(item){
      if(item != undefined){
        for(var i in item.itens){
          if(item.itens[i].id == this.id){
            return true;
          }
        }
      }
      return false;
    },
    getShadows(item){
      if(this.verify(item)){
        return this.shadows;
      }
      return 'rgba(0,0,0,0)';
    },
    getAmountItem(amount){
      if(amount != undefined){
        for(var i in amount['itens']){
          if(amount['itens'][i]['id'] == this.id){
            return amount['itens'][i]['quantidade'];
          }
        }
      }
      return false;
    }
  },
}
</script>

<style scoped>
.contentSection {
  position: relative;
  width: 210px;
  height: 300px;
}
.cardTitle {
  position: relative;
  left: 0px;
  top: 0px;
  width: 200px;
  height: 200px;
  border-radius: 100px;
  background-size: auto 200px;
  background-repeat: no-repeat;
  overflow: hidden;
}
.cardTitle:hover {
  cursor: pointer;
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
  padding-top: 90px;
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
    color: #293141;
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
  display: inline-block;
}
  #buttonAdd {
    display: inline-block;
    width: 98px;
    height: 30px;
    color: #2c3e50;
    background-color: #05e25f;
    color: white;
    border-radius: 15px;
    font-weight: bolder;
    margin: 3px;
  }
  #buttonRemove {
    display: inline-block;
    width: 98px;
    height: 30px;
    color: #2c3e50;
    background-color: #ff544c;
    color: white;
    border-radius: 15px;
    font-weight: bolder;
    margin: 3px;
  }
</style>