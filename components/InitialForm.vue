<template>
  <div>
    <form @submit.prevent="makeReady" class="container mx-auto px-4">
      <div class="grid gap-6 mb-6 md:grid-cols-2">
        <div>
          <label
            for="player0-name"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
            >Player 0 ({{ info.modifiedPlayers[0].wins }} victoire{{
              info.modifiedPlayers[0].wins > 1 ? 's' : ''
            }}) :</label
          >
          <input
            name="player0-name"
            type="text"
            id="player0-name"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
            placeholder="John"
            required
            v-model="info.modifiedPlayers[0].name"
          />
        </div>
        <div>
          <label
            for="player1-name"
            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
            >Player 1 ({{ info.modifiedPlayers[1].wins }} victoire{{
              info.modifiedPlayers[1].wins > 1 ? 's' : ''
            }}) :</label
          >
          <input
            name="player1-name"
            type="text"
            id="player1-name"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
            placeholder="Bernard"
            required
            v-model="info.modifiedPlayers[1].name"
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
            v-model="info.length"
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
            v-model="info.board.rows"
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
            v-model="info.board.columns"
            max="30"
            min="2"
          />
        </div>
      </div>
      <input
        type="submit"
        class="text-white bg-gradient-to-r from-blue-500 via-blue-600 to-blue-700 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
        style="cursor: pointer"
        value="Go !"
      />
      <p v-if="error" class="error">
        Veuillez séléectionner un plus grand nombre de colonnes ou de lignes, ou
        diminuer la puissance.
      </p>
    </form>
  </div>
</template>

<script>
export default {
  name: 'InitialForm',
  props: {
    players: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      error: false,
      info: {
        modifiedPlayers: this.players,
        length: 4,
        board: {
          columns: 7,
          rows: 6,
        },
      },
    }
  },
  methods: {
    makeReady() {
      if (
        parseInt(this.info.length) > parseInt(this.info.board.rows) &&
        parseInt(this.info.length) > parseInt(this.info.board.columns)
      ) {
        this.error = true
      } else {
        this.error = false
        this.$emit('makeReady', this.info)
      }
    },
  },
}
</script>

<style scoped>
.error {
  color: darkred;
}
</style>
