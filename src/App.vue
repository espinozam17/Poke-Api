<template>
  <div >
    <img src="./assets/img/WhatsApp Image 2024-07-12 at 21.10.03.jpeg" alt="pokemon">
    <h1>¿Quién es ese Pokemón?</h1>
    <h5>Pokémons descubiertos: </h5> <h5 class="amarillo">{{ countGuessed }}</h5>
    <div class="container">
      <div class="row row-cols-1 row-cols-md-3 row-cols-lg-4">
        
        

          <div class="col" v-for="(pokemon) in pokemones" :key="pokemon.name">
            <poke-card :pokemon="pokemon" @guess="checkGuess" />
            <p v-if="pokemon.guessed">{{ pokemon.name }}</p>
          </div>

        
      </div>
    </div>


  </div>
</template>

<script>
import axios from 'axios';
import PokeCard from './components/PokeCard.vue';

export default {
  name: 'App',
  components: {
    PokeCard,
  },
  data() {
    return {
      pokemones: [],
      guessInput: '',
    };
  },
  async created() {
    const pokemones = await this.getPokemons();
    this.pokemones = pokemones;
  },
  computed: {
    countGuessed() {
      return this.pokemones.filter(pokemon => pokemon.guessed).length;
    }
  },
  methods: {
    async getPokemons() {
      const random = Math.floor(Math.random() * 1300) 
      const URL_BASE = `https://pokeapi.co/api/v2/pokemon/?limit=20&offset=${random}`;
      
      try {
        const responseApi = await axios.get(URL_BASE);
        const primerLlamado = responseApi.data.results;

        const pokemonResponse = await Promise.all(
          primerLlamado.map(async (pokemon) => {
            const { data } = await axios.get(pokemon.url);
            return { name: data.name, image: data.sprites.other.dream_world.front_default, guessed: false };
          })
        );

        return pokemonResponse;
      } catch (error) {
        console.error("Error fetching Pokémon data: ", error);
        return [];
      }
    },
    checkGuess(pokemon, guess) {
      if (guess.toLowerCase() === pokemon.name.toLowerCase()) {
        pokemon.guessed = true;
        this.guessInput = '';
      } else {
        alert('Nombre incorrecto, inténtalo de nuevo.');
      }
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.amarillo{color: rgb(198, 191, 86);}
</style>