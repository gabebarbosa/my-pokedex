<template>
  <div class="hello">
    <h1>{{ titulo }}</h1>

    <div class="row">

      <div class="col-md-2 m-b-5" v-for="pokemon in pokemons" :key="pokemon.name">
        <ul class="list-group margin-bottom-10">
          <li class="list-group-item">{{pokemon.name}}</li>
        </ul>
      </div>

    </div>
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
.m-b-5 {
    margin-bottom: 5px;
}

</style>
