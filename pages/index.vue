<template>
  <div v-if="!ready">
    <form @submit.prevent="makeReady" class="container mx-auto px-4">
      <div class="grid gap-6 mb-6 md:grid-cols-2">
        <div>
          <label
            for="player0-name"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
            >Player 0 ({{ players[0].wins }} victoire{{
              players[0].wins > 1 ? 's' : ''
            }}) :</label
          >
          <input
            name="player0-name"
            type="text"
            id="player0-name"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
            placeholder="John"
            required
            v-model="players[0].name"
          />
        </div>
        <div>
          <label
            for="player1-name"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
            >Player 1 ({{ players[1].wins }} victoire{{
              players[1].wins > 1 ? 's' : ''
            }}) :</label
          >
          <input
            name="player1-name"
            type="text"
            id="player1-name"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
            placeholder="Bernard"
            required
            v-model="players[1].name"
          />
        </div>
        <div>
          <label
            for="puissance-win"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
            >Puissance pour gagner :
          </label>
          <input
            name="puissance-win"
            type="number"
            id="puissance-win"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
            placeholder="4"
            required
            v-model="length"
            max="20"
            min="2"
          />
        </div>
        <div>
          <label
            for="rows"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
            >Rows :
          </label>
          <input
            name="rows"
            type="number"
            id="rows"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
            placeholder="4"
            required
            v-model="board.rows"
            max="30"
            min="2"
          />
        </div>
        <div>
          <label
            for="columns"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
            >Columns :
          </label>
          <input
            name="columns"
            type="number"
            id="columns"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
            placeholder="4"
            required
            v-model="board.columns"
            max="30"
            min="2"
          />
        </div>
      </div>
      <!--      <label for=""-->
      <!--        >Player 0 ({{ players[0].wins }} victoire{{-->
      <!--          players[0].wins > 1 ? 's' : ''-->
      <!--        }}) :-->
      <!--      </label>-->
      <!--      <input required type="text" v-model="players[0].name" />-->
      <!--      <label for=""-->
      <!--        >Player 1 ({{ players[1].wins }} victoire{{-->
      <!--          players[1].wins > 1 ? 's' : ''-->
      <!--        }}) :-->
      <!--      </label>-->
      <!--      <input required type="text" v-model="players[1].name" />-->
      <!--      <label for="">Puissance pour gagner : </label>-->
      <!--      <input required type="number" v-model="length" max="20" min="2" />-->
      <!--      <label for="">Rows : </label>-->
      <!--      <input required type="number" v-model="board.rows" max="30" min="2" />-->
      <!--      <label for="">Columns : </label>-->
      <!--      <input required type="number" v-model="board.columns" max="30" min="2" />-->
      <input
        type="submit"
        class="text-white bg-gradient-to-r from-blue-500 via-blue-600 to-blue-700 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
        style="cursor: pointer"
        value="Go !"
      />
    </form>
  </div>
  <div v-else class="board container mx-auto px-4">
    <p>A toi de jouer, {{ players[currentPlayer].name }}</p>
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
    <div v-if="winner || draw">
      <h3
        v-if="winner"
        class="mb-4 text-2xl font-extrabold leading-none tracking-tight text-gray-900 md:text-3xl lg:text-4xl dark:text-white"
      >
        Félicitations, {{ players[currentPlayer].name }} !
      </h3>
      <h3
        v-if="draw"
        class="mb-4 text-2xl font-extrabold leading-none tracking-tight text-gray-900 md:text-3xl lg:text-4xl dark:text-white"
      >
        Égalité !
      </h3>

      <button
        @click="restart"
        style="cursor: pointer"
        class="text-white bg-gradient-to-r from-blue-500 via-blue-600 to-blue-700 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
      >
        Recommencer
      </button>
    </div>
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
      already: [],
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
    makeReady() {
      this.board.rows = parseFloat(this.board.rows)
      this.board.columns = parseFloat(this.board.columns)
      this.length = parseFloat(this.length)
      this.ready = true
      this.generateBoard()
    },
    checkLeft(rowParam, colParam) {
      console.log('checkLeft')
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
      console.log('hor', this.checkLeft(row, col) + this.checkRight(row, col))
      return this.checkLeft(row, col) + this.checkRight(row, col)
    },
    checkVertical(row, col) {
      console.log('vert', this.checkUp(row, col) + this.checkDown(row, col))
      return this.checkUp(row, col) + this.checkDown(row, col)
    },
    checkDiagonalUpRightDownLeft(row, col) {
      console.log(
        'diagUR-DL',
        this.checkUp(row, col) + this.checkDown(row, col)
      )
      return this.checkUpRight(row, col) + this.checkDownLeft(row, col)
    },
    checkDiagonalUpLeftDownRight(row, col) {
      console.log(
        'diagUR-DL',
        this.checkUp(row, col) + this.checkDown(row, col)
      )
      return this.checkUpLeft(row, col) + this.checkDownRight(row, col)
    },
    getLowestCase(col) {
      for (let i = this.board.rows - 1; i >= 0; i--) {
        console.log(this.displayedBoard[i][col])
        if (
          this.displayedBoard[i][col] !== 0 &&
          this.displayedBoard[i][col] !== 1
        ) {
          return i
        }
      }
    },
    checkWinner(col) {
      let row = this.getLowestCase(col)
      if (row === undefined || this.winner) {
        return
      }
      if (this.checkDraw()) {
        this.draw = true
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
      console.log(this.displayedBoard)
      let count = 0
      count = this.checkHorizontal(row, col)
      if (count > this.length) {
        this.players[this.currentPlayer].wins++
        this.winner = true
        return true
      }
      count = this.checkVertical(row, col)
      if (count > this.length) {
        this.players[this.currentPlayer].wins++
        this.winner = true
        return true
      }
      count = this.checkDiagonalUpRightDownLeft(row, col)
      if (count > this.length) {
        this.players[this.currentPlayer].wins++
        this.winner = true
        return true
      }
      count = this.checkDiagonalUpLeftDownRight(row, col)
      if (count > this.length) {
        this.players[this.currentPlayer].wins++
        this.winner = true
        return true
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
/* ----------------------------------------------
 * Generated by Animista on 2023-5-10 12:9:52
 * Licensed under FreeBSD License.
 * See http://animista.net/license for more info.
 * w: http://animista.net, t: @cssanimista
 * ---------------------------------------------- */

/**
 * ----------------------------------------
 * animation bounce-in-top
 * ----------------------------------------
 */
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

.row {
  display: flex;
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
}
.player0 {
  background-color: yellow;
  -webkit-animation: bounce-in-top 1.1s both;
  animation: bounce-in-top 1.1s both;
  background-image: url('assets/img/player0.png');
  background-size: cover;
  background-position: center;
}
.player1 {
  background-color: red;
  -webkit-animation: bounce-in-top 1.1s both;
  animation: bounce-in-top 1.1s both;
  background-image: url('assets/img/player1.png');
  background-size: cover;
  background-position: center;
}
</style>
