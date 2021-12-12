<template>
	<section>
    <v-card width="200px" height="200px" @click="select(true)" class="cardColor" :style="{backgroundImage: 'linear-gradient(45deg,' + color1 + ',' + color2 + ')'}">
      <div v-if="selected == id" class="itemSelected">
        Selecionado
      </div>
      <!-- <v-card-actions class="cardActions">
        <button id="buttonAdd" v-if="selected != id">Selecionar</button>
      </v-card-actions> -->
    </v-card>
  </section>
</template>

<script>

export default {
  name: "Card",
  props: {
    color1: String,
    color2: String,
    button: Boolean,
    selected: Number,
    id: Number,
	},
  mounted(){
    this.button = false;
  },
  methods: {
    select(){
      this.$emit('select');
    },
  },
}
</script>

<style scoped>
.cardColor {
  border-radius: 100px;
  overflow: hidden;
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
  padding-top: 85px;
}

.cardActions {
  position: absolute;
  left: 0px;
  bottom: 0px;
  width: 100%;
  height: 40px;
}
  #buttonAdd {
    width: 110px;
    height: 30px;
    color: #2c3e50;
    margin: 5px;
    left: 50%;
    margin-left: -55px;
    position: absolute;
    background-color: green;
    color: white;
    border-radius: 15px;
  }
  #buttonRemove {
    width: 110px;
    height: 30px;
    color: #2c3e50;
    margin: 5px;
    left: 50%;
    margin-left: -55px;
    position: absolute;
    background-color: red;
    color: white;
    border-radius: 15px;
  }
</style>