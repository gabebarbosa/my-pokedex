<template>
  <div class="hello">
    <h1>{{ titulo }}</h1>

    <table class="table">
      <thead>
        <tr>
          <th scope="col">Nome</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="pokemon in pokemons" :key="pokemon.name">
          <td>{{pokemon.name}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Pokemon',
  props: {
    titulo: String
  },
  data () {
    return {
      pokemons: null
    }
  },
  mounted () {
    axios
      .get('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=1000')
      .then(response => (this.pokemons = response.data.results))
  }
}
</script>


<style scoped>

</style>
