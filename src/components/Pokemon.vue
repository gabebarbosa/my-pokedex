<template>
  <div>
    
    <h1 class="text-center">{{ titulo }}</h1>

    <div class="row m-b-5">

      <div class="col-md-3 m-b-5" v-for="pokemon in pokemons" :key="pokemon.name">
        <a 
          class="list-group-item list-group-item-action text-center"
          v-on:click="selecionarPokemon(pokemon)">
          {{pokemon.name}}
        </a>
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

    <PokemonCard :pokemon="{pokemonSelecionado}"/>
  </div>
</template>

<script>
import axios from 'axios'
import PokemonCard from './PokemonCard.vue'

export default {
  name: 'Pokemon',
  components: {
    PokemonCard
  },
  props: {
    titulo: String
  },
  data () {
    return {
      pokemons: null,
      proximo: null,
      anterior: null,
      pokemonSelecionado: null
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
      )
    },
    irAoAnterior() {
      axios
      .get(this.anterior)
      .then(response => (
              this.pokemons = response.data.results,
              this.proximo = response.data.next,
              this.anterior = response.data.previous
              )
      )
    },
    selecionarPokemon (pokemon) {
      axios
      .get(pokemon.url)
      .then(
        response => (
          this.pokemonSelecionado = response.data
        )
      )
    }
  },
  mounted () {
    axios
      .get('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=16')
      .then(response => (
              this.pokemons = response.data.results,
              this.proximo = response.data.next,
              this.anterior = response.data.previous,
              this.pokemonSelecionado = this.selecionarPokemon({url: "https://pokeapi.co/api/v2/pokemon/1/"})
              )
      )
  }
}
</script>


<style scoped>
.m-b-5 {
    margin-bottom: 5px;
}
h1 {
  color: white;
};

</style>
