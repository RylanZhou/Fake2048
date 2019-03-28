<template>
  <div
    class="framework"
    @keyup.left="swipeLeft"
    @keyup.right="swipeRight"
    @keyup.up="swipeUp"
    @keyup.down="swipeDown">
    <div class="info-wrapper">
      <div class="logo">2048</div>
      <div class="info-block">
        <div class="score-best">
          <div class="score">
            <div class="text">SCORE</div>
            <div class="value">{{ score }}</div>
          </div>
          <div class="best">
            <div class="text">BEST</div>
            <div class="value">{{ bestScore }}</div>
          </div>
        </div>
          <div class="menu-leaderboard">
            <div class="menu" @click="showMenu = true">MENU</div>
            <div class="leaderboard">LEADERBOARD</div>
          </div>
      </div>
    </div>
    <div class="hint">
      Your next goal is to get to the {{ goal }} tile!
    </div>
    <div class="game-wrapper">
      <game2048
        :goal="goal"
        @addScore="addScore"
        @gameOver="gameOver()"
        ref="game2048"></game2048>
    </div>
    <transition name="fade">
      <game-menu
        :state="notGameOver"
        v-show="showMenu"
        @hideMenu="showMenu = false"
        @beginNew="beginNew()"
        @goalExceed="this.goal <<= 1"></game-menu>
    </transition>
  </div>
</template>

<script>
import game2048 from '@/components/Game2048.vue';
import gameMenu from '@/components/Menu.vue';

export default {
  components: {
    game2048,
    'game-menu': gameMenu
  },
  data() {
    return {
      goal: 2048,
      score: 0,
      bestScore: 0,
      notGameOver: true,
      showMenu: false
    };
  },
  created() {
    window.onkeyup = (event) => {
      let oEvent = event || window.event;
      switch(oEvent.keyCode) {
        case 37:
          this.$refs.game2048.swipeLeft();
          break;
        case 38:
          this.$refs.game2048.swipeUp();
          break;
        case 39:
          this.$refs.game2048.swipeRight();
          break;
        case 40:
          this.$refs.game2048.swipeDown();
      }
    };
    // if (localStorage.getItem('game2048') !== null) {
    //   this.bestScore = parseInt(localStorage['game2048']);
    //   let pow = Math.floor(Math.log2(this.bestScore));
    //   this.goal = 2048 > 1 << pow ? 2048 : 1 << pow;
    // }
    this.bestScore = parseInt(this.getCookie('game2048'));
    let pow = Math.floor(Math.log2(this.bestScore));
    this.goal = 2048 > 1 << pow ? 2048 : 1 << pow;
  },
  methods: {
    setCookie(name, value, expireDay) {
      let date = new Date();
      date.setDate(date.getDate() + expireDay);
      document.cookie = `${name}=${value};expires=${date.toGMTString()}`;
    },
    getCookie(name) {
      let cookies = unescape(document.cookie).split(';');
      let result = 0;
      for (let each of cookies) {
        let temp = each.split('=');
        if (temp[0] === name) {
          result = temp[1];
          break;
        }
      }
      return result;
    },
    addScore(value) {
      this.score += value;
      this.bestScore = Math.max(this.score, this.bestScore);
      this.setCookie('game2048', this.bestScore, 30);
    },
    gameOver() {
      this.notGameOver = false;
      this.showMenu = true;
    },
    beginNew() {
      this.showMenu = false;
      this.notGameOver = true;
      this.score = 0;
      this.$refs.game2048.init();
    }
  }
};
</script>

<style lang="stylus" scoped>
.framework
  display: inline-block
  margin: 0 auto
  padding: 0 18px
  .info-wrapper
    height: 120px
    margin: 24px 0 34px 0
    .logo
      display: inline-block
      width: 116px  
      line-height: 120px
      vertical-align: top
      text-align: center
      font-size: 32px
      color: white
      font-weight: 700
      background-color: #ecc400
      border-radius: 8px
    .info-block
      display: inline-block
      width: 204px
      margin-left: 18px
      @media screen and (max-width: 360px) {
        margin-left: 10px
      } 
      .score-best
        display: inline-block
        height: 72px
        .score, .best
          display: inline-block
          width: 92px
          height: 72px
          border-radius: 8px
          text-align: center
          background-color: #bbada0
          .text
            margin-top: 12px
            color: #faf1e1
          .value
            margin-top: 6px
            font-size: 22px
            color: white
        .best
          margin-left: 18px
      .menu-leaderboard
        display: inline-block
        height: 34px
        margin-top: 12px
        .menu, .leaderboard
          display: inline-block
          width: 92px
          height: 34px
          line-height: 34px
          border-radius: 8px
          text-align: center
          font-size: 12px
          color: white
          background-color: #ed995b
        .leaderboard
          margin-left: 18px
          font-size: 10px
  .hint
    font-family: "Arial"
  .game-wrapper
    margin-top: 40px
  .fade-enter-active, .fade-leave-active
    transition: all 0.2s
  .fade-enter, .fade-leave-to
    opacity: 0
</style>
