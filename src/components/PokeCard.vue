<template>
  <div class="card">
    <img
      :src="pokemon.image"
      :alt="pokemon.name"
      :class="{ 'blurred': !pokemon.guessed, 'guessed': pokemon.guessed }"
      class="img-fluid"
    />
    <div v-if="!pokemon.guessed">
      <input type="text" v-model="guessInput" @keyup.enter="makeGuess" placeholder="Ingrese el nombre"> <br>
      <button @click="makeGuess">Descubrir</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeCard',
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      guessInput: '',
    };
  },
  methods: {
    makeGuess() {
      this.$emit('guess', this.pokemon, this.guessInput);
    }
  },
};
</script>

<style scoped>
.blurred {
  filter:  grayscale(100%) blur(5px); /* Aplicar un filtro de desenfoque */
}

.guessed {
  filter: none; /* Eliminar cualquier filtro si el Pokémon ha sido adivinado */
}

input {
  margin-right: 5px;
}

button {
  margin-top: 5px;
}
img{height: 12rem;}
.card{height: 20rem;
padding: 2rem;}
</style>