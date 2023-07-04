<template>
  <h1 v-if="!pokemon">
    Espere por favor...
  </h1>

  <div v-else>
    <h1>Quien es este Pokemon?</h1>

    <!-- TODO: img -->
    <PokemonPicture
      :pokemonId="pokemon.id"
      :showPokemon="showPokemon"
    />

    <!-- TODO: opciones -->
    <PokemonOptions
      :pokemons="pokemonArr"
      @selection="checkAnswer"
    />

    <div
      v-if="showAnswer"
      class="answer"
    >
      <h2 class="fade-in">{{ message }}</h2>

      <button
        class=""
        @click="newGame"
      >
        Nuevo Juego
      </button>
    </div>

  </div>
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOptions from '@/components/PokemonOptions.vue'

import getPokemonOptions from '@/helpers/getPokemonOptions'

export default {
  name: 'PokemonPage',
  components: {
    PokemonPicture,
    PokemonOptions
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: {},
      showPokemon: false,
      showAnswer: false,
      message: ''
    }
  },
  methods: {
    async mixPokemonsArray() {
      this.pokemonArr = await getPokemonOptions()
      
      const rndInt = Math.floor(Math.random() * 4)

      this.pokemon = this.pokemonArr[rndInt]
    },
    checkAnswer( selectedId ) {
      this.showPokemon = true
      this.showAnswer = true
      console.log(this.counter)

      if( selectedId === this.pokemon.id ) {
        this.message = `Correcto! Es ${this.pokemon.name}!`
      } else {
        this.message = `Incorrecto! Era ${this.pokemon.name}!`
      }
    },
    newGame() {
      this.showPokemon = false
      this.showAnswer = false
      this.pokemonArr = []
      this.pokemon = null
      this.message = ''

      this.mixPokemonsArray()
    }
  },
  mounted() {
    this.mixPokemonsArray()
  }
}
</script>
