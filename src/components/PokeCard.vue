<template>
  <div class="oculta">
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
  filter: grayscale(100%) blur(5px);

}

.guessed {
  filter: none;
}

input {  
  font-size: 10px;
  
}

button {
  padding: 10px;
  margin-top: 5px;
  background-color: #FFCB03;
  border-color: #335CA6;
  border-width:medium;
  border-radius: 25px;
  color: #335CA6;
  font-weight: bolder;
  
}

img {
  height: 10rem;
  margin-bottom: 1rem;
}

.oculta {
  height: 20rem;
  padding: 0.8rem;
  background-color: brown;
}
</style>