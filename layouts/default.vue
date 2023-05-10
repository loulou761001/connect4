<template>
  <div>
    <header>Super Connect</header>
    <nuxt />
    <footer>Tous droits réservés</footer>
  </div>
</template>

<script>
export default {
  name: 'default',
}
</script>

<style scoped>
header {
  padding: 15px;
  margin-bottom: 15px;
  z-index: 2;
  border-bottom: 1px solid lightgray;
}
footer {
  padding: 15px;
}
</style>
