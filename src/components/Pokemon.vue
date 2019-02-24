<template>
  <div class="hello">
    
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

    <div class="row text-center" v-if="!!pokemonSelecionado" >
      <div class="card col-md-2" style="width: 18rem;">
        <div class="card-body">
          <img 
            class="card-img-top" 
            v-bind:src="pokemonSelecionado.sprites.front_default"
            v-if="!spriteShiny">
          <img 
            class="card-img-top" 
            v-bind:src="pokemonSelecionado.sprites.front_shiny"
            v-else>

            <p class="card-text">#{{pokemonSelecionado.id}} - {{pokemonSelecionado.name}}</p>

            <button 
              v-on:click="verSpriteShiny()" 
              class="btn btn-outline-success btn-sm btn-block"
              v-if="!spriteShiny">Shiny</button>
            <button 
              v-on:click="verSpriteNormal()" 
              class="btn btn-outline-success btn-sm btn-block"
              v-else>Normal</button>
        </div>
      </div>

      <div class="col-md-10 text-left">
        
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
      anterior: null,
      pokemonSelecionado: null,
      spriteShiny: null
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
          this.spriteShiny = false,
          this.pokemonSelecionado = response.data
        )
      )
    },
    verSpriteShiny () {
      this.spriteShiny = true
    },
    verSpriteNormal () {
      this.spriteShiny = false
    }
  },
  mounted () {
    axios
      .get('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=24')
      .then(response => (
              this.spriteShiny = false,
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
