<template>

  <h1 v-if="!pokemon">Espere por favor...</h1>

  <div v-else>
    <h1>¿Quien es este pokémon?</h1>
    <!-- Todo: img -->
      <PokemonPicture
        :pokemon-id="pokemon.id"
        :show-pokemon="showPokemon"
      />
    <!-- Todo: Opciones -->
      <PokemonOptions
        :pokemons="pokemonArr"
        @selection-pokemon="checkAnswer($event)"
      />
      <template v-if="showAnswer">
        <h2 v-if="showAnswer"  class="fade-in">{{ message }}</h2>
        <button @click="newGame">
          Nuevo juego
        </button>
      </template>
  </div>

</template>

<script>

import PokemonPicture from '@/components/PokemonPicture'
import PokemonOptions from '@/components/PokemonOptions'

import getPokemonsOptions from '@/helpers/getPokemonOptions'


export default {
    components: {PokemonPicture, PokemonOptions},
    data(){
      return {
        pokemonArr: [],
        pokemon: null,
        showPokemon: false,
        showAnswer: false,
        message: ''
      }
    },
    methods: {
      async mixPokemonArr(){
        this.pokemonArr = await getPokemonsOptions()
        const rndInt = Math.floor(Math.random() * 4)
        this.pokemon = this.pokemonArr[rndInt]
      },
      checkAnswer(selectedId) {
        this.showPokemon = true
        this.showAnswer = true
        if(selectedId == this.pokemon.id){
          this.message = `Correcto, es ${ this.pokemon.name }`
        } else {
          this.message = `Oops, era ${ this.pokemon.name }`
        }
      },
      newGame() {
        this.showPokemon = false
        this.showAnswer = false
        this.pokemonArr = []
        this.pokemon = null
        this.mixPokemonArr()
      }
    },
    mounted() {
      this.mixPokemonArr()
    },
}

</script>
