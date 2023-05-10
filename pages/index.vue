<template>
  <initial-form v-if="!ready" :players="players" @makeReady="makeReady" />
  <div v-else class="board container mx-auto px-4">
    <h2 class="text-4xl font-extrabold dark:text-white py-3">
      A toi de jouer, {{ players[currentPlayer].name }}
    </h2>
    <div class="row" v-for="(row, rowIndex) in board.rows">
      <div
        v-for="(col, colIndex) in board.columns"
        @click="checkWinner(colIndex)"
        class="case"
      >
        <div
          v-if="
            displayedBoard[rowIndex][colIndex] === 0 ||
            displayedBoard[rowIndex][colIndex] === 1
          "
          class="jeton"
          :class="{
            player0:
              displayedBoard[rowIndex] &&
              displayedBoard[rowIndex][colIndex] === 0,
            player1:
              displayedBoard[rowIndex] &&
              displayedBoard[rowIndex][colIndex] === 1,
          }"
        ></div>
      </div>
    </div>
    <end-screen
      v-if="winner || draw"
      :status="winner ? 'winner' : 'draw'"
      :current-player="players[currentPlayer]"
      @restart="restart"
    />
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'default',

  data() {
    return {
      turn: 1,
      draw: false,
      ready: false,
      winner: false,
      board: {
        columns: 7,
        rows: 6,
      },
      displayedBoard: [],
      currentPlayer: 0,
      players: [
        { name: '', wins: 0 },
        { name: '', wins: 0 },
      ],
      length: 4,
    }
  },
  mounted() {},
  methods: {
    checkDraw() {
      return this.turn >= this.board.columns * this.board.rows
    },
    restart() {
      this.turn = 1
      this.winner = false
      this.draw = false
      this.ready = false
      this.displayedBoard = []
    },
    makeReady(e) {
      this.board.rows = parseFloat(e.board.rows)
      this.board.columns = parseFloat(e.board.columns)
      this.length = parseFloat(e.length)
      this.players = e.modifiedPlayers
      this.ready = true
      this.generateBoard()
    },
    checkLeft(rowParam, colParam) {
      let count = 0

      for (let check = 0; check < this.length; check++) {
        let currentCol = colParam - check
        if (
          currentCol >= 0 &&
          this.displayedBoard[rowParam][currentCol] === this.currentPlayer
        ) {
          count++
        } else {
          return count
        }
      }
      return count
    },
    checkRight(rowParam, colParam) {
      let count = 0

      for (let check = 0; check < this.length; check++) {
        let currentCol = colParam + check

        if (
          currentCol < this.board.columns &&
          this.displayedBoard[rowParam][currentCol] === this.currentPlayer
        ) {
          count++
        } else {
          return count
        }
      }
      return count
    },
    checkDown(rowParam, colParam) {
      let count = 0

      for (let check = 0; check < this.length; check++) {
        let currentRow = rowParam + check

        if (
          currentRow < this.board.rows &&
          this.displayedBoard[currentRow][colParam] === this.currentPlayer
        ) {
          count++
        } else {
          return count
        }
      }
      return count
    },
    checkUp(rowParam, colParam) {
      let count = 0

      for (let check = 0; check < this.length; check++) {
        let currentRow = rowParam - check
        if (
          currentRow >= 0 &&
          this.displayedBoard[currentRow][colParam] === this.currentPlayer
        ) {
          count++
        } else {
          return count
        }
      }
      return count
    },
    checkUpRight(rowParam, colParam) {
      let count = 0

      for (let check = 0; check < this.length; check++) {
        let currentRow = rowParam - check
        let currentCol = colParam + check
        if (
          currentCol < this.board.columns &&
          currentRow >= 0 &&
          this.displayedBoard[currentRow][currentCol] === this.currentPlayer
        ) {
          count++
        } else {
          return count
        }
      }
      return count
    },
    checkUpLeft(rowParam, colParam) {
      let count = 0

      for (let check = 0; check < this.length; check++) {
        let currentRow = rowParam - check
        let currentCol = colParam - check
        if (
          currentCol >= 0 &&
          currentRow >= 0 &&
          this.displayedBoard[currentRow][currentCol] === this.currentPlayer
        ) {
          count++
        } else {
          return count
        }
      }
      return count
    },
    checkDownLeft(rowParam, colParam) {
      let count = 0

      for (let check = 0; check < this.length; check++) {
        let currentRow = rowParam + check
        let currentCol = colParam - check
        if (
          currentCol >= 0 &&
          currentRow < this.board.rows &&
          this.displayedBoard[currentRow][currentCol] === this.currentPlayer
        ) {
          count++
        } else {
          return count
        }
      }
      return count
    },
    checkDownRight(rowParam, colParam) {
      let count = 0

      for (let check = 0; check < this.length; check++) {
        let currentRow = rowParam + check
        let currentCol = colParam + check
        if (
          currentRow < this.board.rows &&
          currentCol < this.board.columns &&
          this.displayedBoard[currentRow][currentCol] === this.currentPlayer
        ) {
          count++
        } else {
          return count
        }
      }
      return count
    },
    checkHorizontal(row, col) {
      return this.checkLeft(row, col) + this.checkRight(row, col)
    },
    checkVertical(row, col) {
      return this.checkUp(row, col) + this.checkDown(row, col)
    },
    checkDiagonalUpRightDownLeft(row, col) {
      return this.checkUpRight(row, col) + this.checkDownLeft(row, col)
    },
    checkDiagonalUpLeftDownRight(row, col) {
      return this.checkUpLeft(row, col) + this.checkDownRight(row, col)
    },
    getLowestCase(col) {
      for (let i = this.board.rows - 1; i >= 0; i--) {
        if (
          this.displayedBoard[i][col] !== 0 &&
          this.displayedBoard[i][col] !== 1
        ) {
          return i
        }
      }
    },
    addWin() {
      this.players[this.currentPlayer].wins++
      this.winner = true
    },
    checkWinner(col) {
      let row = this.getLowestCase(col)
      if (row === undefined || this.winner) {
        return
      }
      if (
        this.displayedBoard[row][col] === 0 ||
        this.displayedBoard[row][col] === 1
      ) {
        return
      }
      this.displayedBoard[row][col] = this.currentPlayer
      this.$set(this.displayedBoard[row], col, this.currentPlayer)
      let count = 0
      count = this.checkHorizontal(row, col)
      if (count > this.length) {
        this.addWin()
        return true
      }
      count = this.checkVertical(row, col)
      if (count > this.length) {
        this.addWin()
        return true
      }
      count = this.checkDiagonalUpRightDownLeft(row, col)
      if (count > this.length) {
        this.addWin()
        return true
      }
      count = this.checkDiagonalUpLeftDownRight(row, col)
      if (count > this.length) {
        this.addWin()
        return true
      }
      if (this.checkDraw()) {
        this.draw = true
        return
      }
      this.turn++
      this.currentPlayer === 1
        ? (this.currentPlayer = 0)
        : (this.currentPlayer = 1)
    },
    generateBoard() {
      for (let row = 0; row < this.board.rows; row++) {
        this.displayedBoard[row] = []
        this.$set(this.displayedBoard, row, [])
        for (let col = 0; col < this.board.columns; col++) {
          this.displayedBoard[row][col] = []
          this.$set(this.displayedBoard[row], col, [])
        }
      }
    },
  },
}
</script>

<style scoped>
@-webkit-keyframes bounce-in-top {
  0% {
    -webkit-transform: translateY(-500px);
    transform: translateY(-500px);
    -webkit-animation-timing-function: ease-in;
    animation-timing-function: ease-in;
    opacity: 0;
  }
  38% {
    -webkit-transform: translateY(0);
    transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
    animation-timing-function: ease-out;
    opacity: 1;
  }
  55% {
    -webkit-transform: translateY(-65px);
    transform: translateY(-65px);
    -webkit-animation-timing-function: ease-in;
    animation-timing-function: ease-in;
  }
  72% {
    -webkit-transform: translateY(0);
    transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
    animation-timing-function: ease-out;
  }
  81% {
    -webkit-transform: translateY(-28px);
    transform: translateY(-28px);
    -webkit-animation-timing-function: ease-in;
    animation-timing-function: ease-in;
  }
  90% {
    -webkit-transform: translateY(0);
    transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
    animation-timing-function: ease-out;
  }
  95% {
    -webkit-transform: translateY(-8px);
    transform: translateY(-8px);
    -webkit-animation-timing-function: ease-in;
    animation-timing-function: ease-in;
  }
  100% {
    -webkit-transform: translateY(0);
    transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
    animation-timing-function: ease-out;
  }
}
@keyframes bounce-in-top {
  0% {
    -webkit-transform: translateY(-500px);
    transform: translateY(-500px);
    -webkit-animation-timing-function: ease-in;
    animation-timing-function: ease-in;
    opacity: 0;
  }
  38% {
    -webkit-transform: translateY(0);
    transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
    animation-timing-function: ease-out;
    opacity: 1;
  }
  55% {
    -webkit-transform: translateY(-65px);
    transform: translateY(-65px);
    -webkit-animation-timing-function: ease-in;
    animation-timing-function: ease-in;
  }
  72% {
    -webkit-transform: translateY(0);
    transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
    animation-timing-function: ease-out;
  }
  81% {
    -webkit-transform: translateY(-28px);
    transform: translateY(-28px);
    -webkit-animation-timing-function: ease-in;
    animation-timing-function: ease-in;
  }
  90% {
    -webkit-transform: translateY(0);
    transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
    animation-timing-function: ease-out;
  }
  95% {
    -webkit-transform: translateY(-8px);
    transform: translateY(-8px);
    -webkit-animation-timing-function: ease-in;
    animation-timing-function: ease-in;
  }
  100% {
    -webkit-transform: translateY(0);
    transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
    animation-timing-function: ease-out;
  }
}

.board {
  margin-bottom: 15px;
}
.row {
  display: flex;
}
.end {
  margin-top: 15px;
}
.case {
  outline: 1px solid black;
  height: 60px;
  width: 60px;
  padding: 5px;
  position: relative;
}
.case::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 3;
  background-image: url('assets/img/case.svg');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
.jeton {
  height: 100%;
  width: 100%;
  border-radius: 50%;
  background-size: cover;
  background-position: center;
  -webkit-animation: bounce-in-top 1.1s both;
}
.player0 {
  background-color: yellow;
  animation: bounce-in-top 1.1s both;
  background-image: url('assets/img/player0.png');
}
.player1 {
  background-color: red;
  animation: bounce-in-top 1.1s both;
  background-image: url('assets/img/player1.png');
}
</style>
