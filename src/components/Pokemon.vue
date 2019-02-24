<template>
  <div class="hello">
    
    <h1 class="text-center">{{ titulo }}</h1>

    <div class="row m-b-5">

      <div class="col-md-2 m-b-5" v-for="pokemon in pokemons" :key="pokemon.name">
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

    <div class="row text-center" v-if="!!pokemonSelecionado" >
      <div class="card col-md-2" style="width: 18rem;">
        <img class="card-img-top" v-bind:src="pokemonSelecionado.sprites.front_default">
      </div>

      <div class="col-md-10 text-left">
        <span>#{{pokemonSelecionado.order}}</span> <br>
        <span>{{pokemonSelecionado.name}}</span>
      </div>


      <!-- <div class="col-md-2">
        <div>
          <img v-bind:src="pokemonSelecionado.sprites.front_default" class="rounded">
        </div>
      </div>
      
      {{pokemonSelecionado.id}} - {{pokemonSelecionado.name}} -->
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
      .get('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=18')
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

</style>
