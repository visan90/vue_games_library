<template>
  <div id="GamesLibrary">
    <GamesList />
  </div>
</template>

<script>
import GamesList from './components/GamesList.vue'

export default {
  name: 'app',
  components: {
    GamesList
  }
}
</script>

<style>

</style>
