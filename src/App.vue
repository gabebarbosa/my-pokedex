<template>
  <body>
    <div id="app" class="container">
      <Pokemon titulo="Pokemons"/>
    </div>
  </body>
  
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
