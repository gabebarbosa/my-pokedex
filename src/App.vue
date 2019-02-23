<template>
  <div id="app">
    <Pokemon titulo="lista de pokemons"/>
  </div>
</template>

<script>
import Pokemon from './components/Pokemon.vue'

export default {
  name: 'app',
  components: {
    Pokemon
  }
}
</script>

<style>
  @import '../node_modules/bootstrap/dist/css/bootstrap.css';
</style>
