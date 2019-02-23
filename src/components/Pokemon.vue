<template>
  <div class="hello">
    
    <h1 class="text-center">{{ titulo }}</h1>

    <div class="row m-b-5">

      <div class="col-md-2 m-b-5" v-for="pokemon in pokemons" :key="pokemon.name">
        <ul class="list-group margin-bottom-10">
          <li class="list-group-item">{{pokemon.name}}</li>
        </ul>
      </div>

    </div>

    <div class="row">

      <div class="col-md-6">
        <button 
          type="button" 
          class="btn btn-primary btn-lg float-right"
          v-on:click="irAoAnterior()"
          v-bind:disabled="!anterior">
            Anterior
        </button>
      </div>
      <div class="col-md-6">
        <button 
          type="button" 
          class="btn btn-primary btn-lg float-left" 
          v-on:click="irAoProximo()"
          v-bind:disabled="!proximo">
            Próximo
        </button>
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
      pokemons: null,
      proximo: null,
      anterior: null
    }
  },
  methods: {
    irAoProximo() {
      axios
      .get(this.proximo)
      .then(response => (
              this.pokemons = response.data.results,
              this.proximo = response.data.next,
              this.anterior = response.data.previous
              ),
      this.pokemons.forEach(pokemon => {
        this.lerPokemonCompleto(pokemon)  
      })
      )
    },
    irAoAnterior() {
      axios
      .get(this.anterior)
      .then(response => (
              this.pokemons = response.data.results,
              this.proximo = response.data.next,
              this.anterior = response.data.previous
              ),
      this.pokemons.forEach(pokemon => {
        this.lerPokemonCompleto(pokemon)  
      })
      )
    },
    lerPokemonCompleto (pokemon) {
      axios
      .get(pokemon.url)
      .then(
        response => (
          pokemon = response.data
        )
      )
    }
  },
  mounted () {
    axios
      .get('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=18')
      .then(response => (
              this.pokemons = response.data.results,
              this.proximo = response.data.next,
              this.anterior = response.data.previous
              )
      )
  }
}
</script>


<style scoped>
.m-b-5 {
    margin-bottom: 5px;
}

</style>
