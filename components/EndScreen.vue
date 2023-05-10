<template>
  <div class="end">
    <h3
      v-if="status === 'winner'"
      class="mb-4 text-2xl font-extrabold leading-none tracking-tight text-gray-900 md:text-3xl lg:text-4xl dark:text-white"
    >
      Félicitations, {{ currentPlayer.name }} !
    </h3>
    <h3
      v-else-if="status === 'draw'"
      class="mb-4 text-2xl font-extrabold leading-none tracking-tight text-gray-900 md:text-3xl lg:text-4xl dark:text-white"
    >
      Égalité !
    </h3>

    <button
      @click="$emit('restart')"
      style="cursor: pointer"
      class="text-white bg-gradient-to-r from-blue-500 via-blue-600 to-blue-700 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
    >
      Recommencer
    </button>
  </div>
</template>

<script>
export default {
  name: 'EndScreen',
  props: {
    status: {
      type: String,
      required: true,
    },
    currentPlayer: {
      type: Object,
      required: true,
    },
  },
}
</script>

<style scoped></style>
