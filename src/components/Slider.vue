<template>
	<div class="mainContent">
    <transition-group name="fade" tag="div" class="a">
      <div v-for="i in [currentIndex]" :key="i">
        <center>
          <img :src="getImageSrc(currentImg)" />
        </center>
      </div>
    </transition-group>
    <a class="prev" @click="prev" href="#">&#10094;</a>
    <a class="next" @click="nextClick" href="#">&#10095;</a>
  </div>
</template>

<script>
export default {
  name: "Slider",
  data() {
    return {
      timer: null,
      currentIndex: 0,
      jump: false,
      images: []
    };
  },

  props: {
    imagesSlider: [],
  },

  mounted() {
    var imagesSlider = JSON.parse(JSON.stringify(this.imagesSlider));
    for(var i=0; i<imagesSlider['imgs'].length; i++){
      this.images.push(imagesSlider['imgs'][i]);
    }
    this.startSlide();
  },

  methods: {
    startSlide() {
      this.timer = setInterval(this.next, 8000);
    },

    next() {
      if(!this.jump){
        this.currentIndex += 1;
      }
      this.jump = false;
    },
    prev() {
      this.jump = true;
      this.currentIndex -= 1;
    },
    nextClick() {
      this.jump = true;
      this.currentIndex += 1;
    },
    getImageSrc(url) {
      if(url != undefined){
        return require('@/' + url);
      }
      return require('@/assets/slider/default.png');
    }
  },

  computed: {
    currentImg() {
      return this.images[Math.abs(this.currentIndex) % this.images.length];
    }
  }
};
</script>

<style scoped>
.mainContent {
  width: 100%;
  height: 80vh;
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

img {
  height: 80vh;
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
</style>