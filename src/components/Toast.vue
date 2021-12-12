<template>
  <div class="toast-wrapper">
    <div class="toast" :style="{'background-color': color}">
      {{ text }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Toast",
  props: {
    color: String,
    text: String,
  }
};
</script>

<style>
  .toast-wrapper {
    position: fixed;
    width: 90%;
    top: 20px;
    left: 5%;
  }
  .toast {
    padding: 20px;
    color: white;
    border-radius: 10px;
    box-shadow: 1px 3px 5px rgba(0,0,0,0.2);
    max-width: 400px;
    margin: 0 auto;
  }
</style>