<template>
  <div class="menu">
    <h1 class="title" v-if="state === true">Menu</h1>
    <h1 class="title" v-else>Game Over!</h1>
    <button class="button" v-if="state === true" @click="hideMenu()">KEEP GOING</button>
    <button class="button" @click="beginNew()">NEW GAME</button>
    <button class="button">HOW TO PLAY</button>
    <div class="footer">
      <p>Game Version: v0.1</p>
      <p>RylanZ 2018 &copy; All rights reserved.</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    state: {
      type: Boolean,
      default: true
    }
  },
  methods: {
    hideMenu() {
      this.$emit('hideMenu');
    },
    beginNew() {
      this.$emit('beginNew');
    }
  }
};
</script>

<style lang="stylus" scoped>
.menu
  position: absolute
  top: 0
  left: 0
  z-index: 200
  width: 100vw
  height: 100vh
  padding-top: 30px
  text-align: center
  background-color: #faf8ef
  .title
    font-size: 50px
    font-weight: 700
    color: #776e65
  .button
    display: block
    margin: 0 auto
    margin-top: 50px
    width: 250px
    height: 80px
    border-radius: 6px
    outline: none
    border: none
    font-size: 26px
    font-weight: 700
    color: #fff
    background-color: #bbada0
  .footer
    position: absolute
    left: 0
    right: 0
    bottom: 40px
    font-size: 12px
    color: #e1e1e1
</style>
