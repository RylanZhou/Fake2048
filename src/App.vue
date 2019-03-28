<template>
  <div id="app">
    <framework></framework>
  </div>
</template>

<script>
import framework from "@/views/Framework/index.vue";

export default {
  components: {
    framework
  }
};
</script>

<style>
@font-face {
  font-family: 'Franklin Gothic';
  src: url('./assets/fonts/Franklin Gothic.ttf') format('truetype');
}

*{
  font-family: 'Franklin Gothic';
  margin: 0;
  padding: 0;
}
html, body {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
  overflow: hidden;
  background-color: #faf8ef;
}
#app{
  height: 100%;
  display: flex;
  justify-content: center;
}
</style>
