<template>
  <transition name="fade">
    <div class="modal" v-if="show">
      <div class="modalBackdrop" @click="closeModal()"/>
      <div class="modalDialog">
        <p class="close" @click="closeModal()">x</p>
        <div class="modalHeader">
          <transition-group name="fade" tag="div" class="a">
            <div v-for="i in [currentIndex]" :key="i">
              <center>
                <img :src="require('@/'+currentImg)" />
              </center>
            </div>
          </transition-group>
          <a class="prev" @click="prev" href="#">&#10094;</a>
          <a class="next" @click="nextClick" href="#">&#10095;</a>
        </div>
        <div class="modalBody">
          <h3>{{jsonInfo['nome'] || jsonInfo['quantidade'] + ' itens'}}</h3>
          <h5 v-if="jsonInfo['valor']">R$ {{jsonInfo['valor']}},00</h5>
          <p>{{jsonInfo['descricao']}}</p>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "ModalEditCategories",
  data() {
    return {
      show: false,
      jsonInfo: '',
      timer: null,
      currentIndex: 0,
      jump: false,
      images: []
    };
  },
  methods: {
    startSlide: function() {
      this.timer = setInterval(this.next, 8000);
    },

    next: function() {
      if(!this.jump){
        this.currentIndex += 1;
      }
      this.jump = false;
    },
    prev: function() {
      this.jump = true;
      this.currentIndex -= 1;
    },
    nextClick: function() {
      this.jump = true;
      this.currentIndex += 1;
    },

    closeModal() {
      this.show = false;
    },

    openModal(type, item) {
      this.images = [];
      this.jsonInfo = JSON.parse(JSON.stringify(item));
      for(var i=0; i<this.jsonInfo['imagens'].length; i++){
        this.images.push(this.jsonInfo['imagens'][i]);
      }
      this.show = true;
    }
  },
  computed: {
    currentImg: function() {
      return this.images[Math.abs(this.currentIndex) % this.images.length];
    }
  }
};
</script>


<style lang="scss">
.modal {
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
    width: 70vw;
    max-width: 600px;
    margin: 50px auto;
    display: flex;
    min-height: 220px;
    flex-direction: column;
    border-radius: 5px;
    z-index: 2;
    overflow: hidden;
    padding: 0px 0px 0px 0px;
    @media screen and (max-width: 992px) {
      width: 90%;
    }
  }
}
.close {
  position: absolute;
  z-index: 101;
  right: 10px;
  top: 0px;
  color: black;
  cursor: pointer;
  width: 10px;
  height: 20px;
}
.mainContent {
  width: 100%;
  height: 80vh;
  position: relative;
  overflow: hidden;
  border-radius: 5px;
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

.modalHeader {
  width: 100%;
  max-width: 600px;
  height: 400px;
  overflow: hidden;
  border-radius: 5px;
}
img {
  height: 400px;
  width: auto;
}

.prev, .next {
  cursor: pointer;
  position: absolute;
  top: 50%;
  margin-top: -40px;
  width: auto;
  padding: 16px;
  color: #bbbbbb;
  font-weight: bold;
  font-size: 18px;
  transition: 0.7s ease;
  border-radius: 0 4px 4px 0;
  text-decoration: none;
  user-select: none;
}

.next {
  right: 0;
}

.prev {
  left: 0;
}

.prev:hover, .next:hover {
  background-color: rgba(0,0,0,0.9);
}

.modalBody {
  text-align: center;
}
h3 {
  margin: 15px 5px 10px 5px;
  color: #293141;
}
h5 {
  margin: 5px;
  color: #293141;
}
p {
  margin: 5px;
  color: #707070;
}
</style>