<template>
  <v-touch
    @swipeleft="swipeLeft"
    @swiperight="swipeRight"
    @swipeup="swipeUp"
    @swipedown="swipeDown">
    <div class="game2048">
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[0][0]}`">{{ classMatrix[0][0] === 0 ? '': classMatrix[0][0] }}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[0][1]}`">{{ classMatrix[0][1] === 0 ? '': classMatrix[0][1]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[0][2]}`">{{ classMatrix[0][2] === 0 ? '': classMatrix[0][2]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[0][3]}`">{{ classMatrix[0][3] === 0 ? '': classMatrix[0][3]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[1][0]}`">{{ classMatrix[1][0] === 0 ? '': classMatrix[1][0]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[1][1]}`">{{ classMatrix[1][1] === 0 ? '': classMatrix[1][1]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[1][2]}`">{{ classMatrix[1][2] === 0 ? '': classMatrix[1][2]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[1][3]}`">{{ classMatrix[1][3] === 0 ? '': classMatrix[1][3]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[2][0]}`">{{ classMatrix[2][0] === 0 ? '': classMatrix[2][0]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[2][1]}`">{{ classMatrix[2][1] === 0 ? '': classMatrix[2][1]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[2][2]}`">{{ classMatrix[2][2] === 0 ? '': classMatrix[2][2]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[2][3]}`">{{ classMatrix[2][3] === 0 ? '': classMatrix[2][3]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[3][0]}`">{{ classMatrix[3][0] === 0 ? '': classMatrix[3][0]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[3][1]}`">{{ classMatrix[3][1] === 0 ? '': classMatrix[3][1]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[3][2]}`">{{ classMatrix[3][2] === 0 ? '': classMatrix[3][2]}}</div>
      </div>
      <div class="block">
        <div class="tile" :class="`t-${classMatrix[3][3]}`">{{ classMatrix[3][3] === 0 ? '': classMatrix[3][3]}}</div>
      </div>
    </div>
  </v-touch>
</template>

<script>
export default {
  props: {
    goal: {
      type: Number,
      default: 2048
    }
  },
  data() {
    return {
      classMatrix: [
        [0, 0, 0, 0], 
        [0, 0, 0, 0], 
        [0, 0, 0, 0], 
        [0, 0, 0, 0]
      ],
      gameOver: false,
      ratio: 0.9
    };
  },
  created() {
    this.$nextTick(() => {
      this.init();
    });
  },
  methods: {
    init() {
      this.gameOver = false;
      this.$set(this.classMatrix, 0, [0, 0, 0, 0]);
      this.$set(this.classMatrix, 1, [0, 0, 0, 0]);
      this.$set(this.classMatrix, 2, [0, 0, 0, 0]);
      this.$set(this.classMatrix, 3, [0, 0, 0, 0]);
      this.generateNew();
      this.generateNew();
    },
    randomRowColumn() {
      let row = Math.floor(Math.random() * 4);
      let column = Math.floor(Math.random() * 4);
      return {row: row, column: column};
    },
    randomNumber() {
      let number = [2, 4];
      if (document.getElementsByClassName('t-512').length > 0) {
        this.ratio = 0.7;  
      }
      if (document.getElementsByClassName('t-2048').length > 0) {
        this.ratio = 0.5;
      }
      if (document.getElementsByClassName('t-4096').length > 0) {
        this.ratio = 0.2;
      }
      let choose = Math.random() > this.ratio ? 1 : 0;
      return number[choose];
    },
    generateNew() {
      if (this.gameOver === true) {
        return;
      }
      let coordinate = this.randomRowColumn();
      while (this.classMatrix[coordinate.row][coordinate.column] !== 0) {
        coordinate = this.randomRowColumn();
      }
      this.$set(this.classMatrix[coordinate.row], coordinate.column, this.randomNumber());
      if (document.getElementsByClassName(`t-${this.goal}`).length !== 0) {
        this.$emit('goalExceed');
      }
      setTimeout(() => {
        if (this.judgeOver() === true) {
          this.gameOver = true;
          this.$emit('gameOver');
        }
      }, 200);
    },
    judgeOver() {
      let tilesAvailable = document.getElementsByClassName('t-0');
      if (tilesAvailable.length !== 0) {
        return false;
      }
      for (let i = 0; i < 4; i ++) {
        for (let j = 0; j < 4; j ++) {
          if (this.classMatrix[i][j] !== 0) {
            if (i - 1 >= 0) {
              if (this.classMatrix[i][j] === this.classMatrix[i - 1][j]) {
                return false;
              }
            }
            if (j - 1 >= 0) {
              if (this.classMatrix[i][j] === this.classMatrix[i][j - 1]) {
                return false;
              }
            }
            if (i + 1 <= 3) {
              if (this.classMatrix[i][j] === this.classMatrix[i + 1][j]) {
                return false;
              }
            }
            if (j + 1 <= 3) {
              if (this.classMatrix[i][j] === this.classMatrix[i][j + 1]) {
                return false;
              }
            }
          }
        }
      }
      // None of the adjacent tiles of any tiles has the same number, game over.
      return true;
    },
    swipeLeft() {
      if (this.gameOver === true) {
        return;
      }
      let tiles = document.getElementsByClassName('tile');
      // Whether there are tiles that can move
      let flag = false;
      // Iterate sequence (Up -> Down, Left -> Right): 0 4 8 12 1 5 9 13 2 6 10 14 3 7 11 15
      for (let column = 0; column < 4; column ++) {
        for (let row = 0; row < 4; row ++) {
          let currentTile = column + 4 * row;
          let className =  tiles[currentTile].className;
          // If this tile has number on it
          if (className.indexOf('t-0') === -1) { 
            // The tiles that can move left should be at least not at column 0.
            if (column !== 0) {
              // Calculate how many tiles can it move.
              let span = 0;
              while (span < column) {
                if (this.classMatrix[row][column - span - 1] !== 0) {
                  if (this.classMatrix[row][column - span - 1] === this.classMatrix[row][column]) {
                    span ++;
                    this.$emit('addScore', 2 * this.classMatrix[row][column]);
                  }
                  break;
                }
                span ++;
              }
              if (span !== 0) {
                flag = true;
                tiles[currentTile].style.left = 0;
                tiles[currentTile].style.left = tiles[currentTile].offsetLeft - 80 * span + 'px';
                this.$set(this.classMatrix[row], column - span, 
                          this.classMatrix[row][column] + this.classMatrix[row][column - span]);
                this.$set(this.classMatrix[row], column, 0);
                setTimeout(() => {
                  tiles[currentTile].style.left = 0;
                }, 100);
              }
            }
          }
        }
      }
      // Only when there is tile moved should generateNew() be invoked.
      if (flag === true) {
        setTimeout(() => {
          this.generateNew();
        }, 200);
      }
    },
    swipeRight() {
      if (this.gameOver === true) {
        return;
      }
      let tiles = document.getElementsByClassName('tile');
      // Whether there are tiles that can move
      let flag = false;
      // Iterate sequence (Up -> Down, Right -> Left): 3 7 11 15 2 6 10 14 1 5 9 13 0 4 8 12
      for (let column = 3; column >= 0; column --) {
        for (let row = 0; row < 4; row ++) {
          let currentTile = column + 4 * row;
          let className =  tiles[currentTile].className;
          // If this tile has number on it
          if (className.indexOf('t-0') === -1) { 
            // If it is not at column 3, it could move right.
            if (column !== 3) {
              // Calculate how many tiles can it move.
              let span = 0;
              while (span < 3 - column) {
                if (this.classMatrix[row][column + span + 1] !== 0) {
                  if (this.classMatrix[row][column + span + 1] === this.classMatrix[row][column]) {
                    span ++;
                    this.$emit('addScore', 2 * this.classMatrix[row][column]);
                  }
                  break;
                }
                span ++;
              }
              if (span !== 0) {
                flag = true;
                tiles[currentTile].style.left = 0;
                tiles[currentTile].style.left = tiles[currentTile].offsetLeft + 80 * span + 'px';
                this.$set(this.classMatrix[row], column + span, 
                          this.classMatrix[row][column] + this.classMatrix[row][column + span]);
                this.$set(this.classMatrix[row], column, 0);
                setTimeout(() => {
                  tiles[currentTile].style.left = 0;
                }, 100);
              }
            }
          }
        }
      }
      if (flag === true) {
        setTimeout(() => {
          this.generateNew();
        }, 200);
      }
    },
    swipeUp() {
      if (this.gameOver === true) {
        return;
      }
      let tiles = document.getElementsByClassName('tile');
      // Whether there are tiles that can move
      let flag = false;
      // Iterate sequence (Left -> Right, Up -> Down): 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15
      for (let row = 0; row < 4; row ++) {
        for (let column = 0; column < 4; column ++) {
          let currentTile = column + 4 * row;
          let className =  tiles[currentTile].className;
          // If this tile has number on it
          if (className.indexOf('t-0') === -1) { 
            // If it is not at row 0, it could move up.
            if (row !== 0) {
              // Calculate how many tiles can it move.
              let span = 0;
              while (span < row) {
                if (this.classMatrix[row - span - 1][column] !== 0) {
                  if (this.classMatrix[row - span - 1][column] === this.classMatrix[row][column]) {
                    span ++;
                    this.$emit('addScore', 2 * this.classMatrix[row][column]);
                  }
                  break;
                }
                span ++;
              }
              if (span !== 0) {
                flag = true;
                tiles[currentTile].style.top = 0;
                tiles[currentTile].style.top = tiles[currentTile].offsetTop - 80 * span + 'px';
                this.$set(this.classMatrix[row - span], column, 
                          this.classMatrix[row][column] + this.classMatrix[row - span][column]);
                this.$set(this.classMatrix[row], column, 0);
                setTimeout(() => {
                  tiles[currentTile].style.top = 0;
                }, 100);
              }
            }
          }
        }
      }
      // If there are tiles that moved
      if (flag === true) {
        setTimeout(() => {
          this.generateNew();
        }, 200);
      }
    },
    swipeDown() {
      if (this.gameOver === true) {
        return;
      }
      let tiles = document.getElementsByClassName('tile');
      // Whether there are tiles that can move
      let flag = false;
      // Iterate sequence (Left -> Right, Down -> Up): 12 13 14 15 8 9 10 11 4 5 6 7 0 1 2 3
      for (let row = 3; row >= 0; row --) {
        for (let column = 0; column < 4; column ++) {
          let currentTile = column + 4 * row;
          let className =  tiles[currentTile].className;
          // If this tile has number on it
          if (className.indexOf('t-0') === -1) { 
            // If it is not at row 3, it could move down.
            if (row !== 3) {
              // Calculate how many tiles can it move.
              let span = 0;
              while (span < 3 - row) {
                if (this.classMatrix[row + span + 1][column] !== 0) {
                  if (this.classMatrix[row + span + 1][column] === this.classMatrix[row][column]) {
                    span ++;
                    this.$emit('addScore', 2 * this.classMatrix[row][column]);
                  }
                  break;
                }
                span ++;
              }
              if (span !== 0) {
                flag = true;
                tiles[currentTile].style.top = 0;
                tiles[currentTile].style.top = tiles[currentTile].offsetTop + 80 * span + 'px';
                this.$set(this.classMatrix[row + span], column, 
                          this.classMatrix[row][column] + this.classMatrix[row + span][column]);
                this.$set(this.classMatrix[row], column, 0);
                setTimeout(() => {
                  tiles[currentTile].style.top = 0;
                }, 100);
              }
            }
          }
        }
      }
      if (flag === true) {
        setTimeout(() => {
          this.generateNew();
        }, 200);
      }
    },
  }
};
</script>

<style lang="stylus" scoped>
@import "../common/color"

.game2048
  width: 330px
  height: 350px
  margin: 0 auto
  border-radius: 8px
  background-color: #bbac9f
  .block
    display: inline-block
    position: relative
    width: 70px
    height: 70px
    margin-left: 10px
    margin-top: 10px
    border-radius: 4px
    text-align: center
    background-color: #ccc0b4
    .tile
      position: absolute
      z-index: 100
      width: 70px
      height: 70px
      vertical-align: top
      line-height: 70px
      border-radius: 4px
      font-size: 36px
      font-family: 'Franklin Gothic'
      transition: all 0.2s ease-in-out
      &.t-0
        margin: 0 auto
        background-color: #ccc0b4
      &.t-2
        tile-color(2)
      &.t-4
        tile-color(4)
      &.t-8
        tile-color(8)
      &.t-16
        tile-color(16)
      &.t-32
        tile-color(32)
      &.t-64
        tile-color(64)
      &.t-128
        tile-color(128)
      &.t-256
        tile-color(256)
      &.t-512
        tile-color(512)
      &.t-1024
        tile-color(1024)
      &.t-2048
        tile-color(2048)
      &.t-4096
        tile-color(4096)
</style>
