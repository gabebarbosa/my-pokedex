<template>
  <div>
    
    <h1 class="text-center"></h1>


    <div class="row text-center" v-if="!!pokemon.pokemonSelecionado" >
      <div class="card col-md-2" style="width: 18rem;">
        <div class="card-body">
          <img 
            class="card-img-top" 
            v-bind:src="pokemon.pokemonSelecionado.sprites.front_default"
            v-if="!spriteShiny">
          <img 
            class="card-img-top" 
            v-bind:src="pokemon.pokemonSelecionado.sprites.front_shiny"
            v-else>

            <p class="card-text">#{{pokemon.pokemonSelecionado.id}} - {{pokemon.pokemonSelecionado.name}}</p>

            <button 
              class="btn btn-outline-success btn-sm btn-block"
              v-on:click="verSpriteShiny()"
              v-if="!spriteShiny">Shiny</button>
            <button 
              class="btn btn-outline-success btn-sm btn-block"
              v-on:click="verSpriteNormal()"
              v-else>Normal</button>
        </div>
      </div>

      <div class="col-md-10 text-left">
        
      </div>
    </div>

  </div>
</template>

<script>
// import axios from 'axios'

export default {
  name: 'PokemonCard',
  props: {
    pokemon: Object
  },
  data () {
    return {
      spriteShiny: null
    }
  },
  mounted () {
    this.spriteShiny = false
  },
  methods: {
    verSpriteShiny () {
      this.spriteShiny = true
    },
    verSpriteNormal () {
      this.spriteShiny = false
    }
  }
}
</script>

<style scoped>

</style>
