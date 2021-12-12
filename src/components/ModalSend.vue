<template>
  <transition name="fade">
    <div class="modalSend" v-if="show">
      <div class="modalSendBackdrop" @click="closeModal()"/>
      <div class="modalSendDialog">
        <h3>Poucos produtos selecionados!</h3>
        <p>Tem certeza que você deseja adicionar apenas esses itens a sua cesta? Você ainda pode adicionar mais produtos</p>

        <div class="footerSend">
          <button class="buttonAddSend" @click="closeModal()">Adicionar mais</button>
          <button class="buttonFinishSend" @click="send()">Finalizar pedido</button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "ModalSend",
  data() {
    return {
      show: false
    };
  },
  methods: {
    closeModal() {
      this.show = false;
    },

    send(){
      this.$emit('send');
      this.closeModal();
    },

    openModal() {
      this.show = true;
    }
  }
};
</script>


<style lang="scss">
.modalSend {
  overflow-x: hidden;
  overflow-y: auto;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 9;
  &Backdrop {
    background-color: rgba(0, 0, 0, 0.3);
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 1;
  }
  &Dialog {
    background-color: #ffffff;
    position: relative;
    width: 60%;
    max-width: 400px;
    margin: 50px auto;
    display: flex;
    min-height: 220px;
    flex-direction: column;
    border-radius: 5px;
    z-index: 2;
    padding: 0px 0px 0px 0px;
    @media screen and (max-width: 992px) {
      width: 90%;
    }
  }
}
.closeSend {
  position: absolute;
  z-index: 101;
  right: 10px;
  top: 0px;
  color: black;
  cursor: pointer;
  width: 10px;
  height: 20px;
}
.mainContentSend {
  width: 100%;
  height: auto;
  position: relative;
  overflow: hidden;
}
.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s ease;
  overflow: hidden;
  visibility: visible;
  position: absolute;
  width:100%;
  opacity: 1;
}
.fade-enter,
.fade-leave-to {
  visibility: hidden;
  width:100%;
  opacity: 0;
}
h3 {
  text-align: center;
}
p {
  text-align: center;
}
.footerSend {
  width: 100%;
  display: inline-block;
  text-align: center;
}
.buttonAddSend {
  position: relative;
  display: inline-block;
  width: 90px;
  height: 30px;
  background-color: #05e25f;
  border-radius: 30px;
  color: white;
  padding: 3px;
}
.buttonFinishSend {
  position: relative;
  display: inline-block;
  width: 150px;
  height: 30px;
  background-color: white;
  border-radius: 30px;
  color: #05e25f;
  padding: 3px;
}
</style>