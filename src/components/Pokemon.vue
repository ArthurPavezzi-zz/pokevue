<template>
  <div class="container">
    <h1>PokéVue</h1>
    <pokemon-list :imageUrl="imageUrl" :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl"/>
    <pokemon-details v-if="showDetails"
                     :pokemonUrl="pokemonUrl" :imageUrl="imageUrl"
                     @closeDetails="closeDetails"/>
  </div>
</template>

<script>
import PokemonList from './PokemonList.vue';
import PokemonDetails from './PokemonDetails.vue';

export default {
  data: () => {
    return {
      imageUrl: 'https://pokeres.bastionbot.org/images/pokemon/',
      apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
      pokemonUrl: '',
      showDetails: false
    }
  },
  components: {
    PokemonList,
    PokemonDetails
  },
  methods: {
    setPokemonUrl(url) {
      this.pokemonUrl = url;
      this.showDetails = true;
    },
    closeDetails() {
      this.pokemonUrl = '';
      this.showDetails = false;
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat');

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  min-height: 100vh;
  background: darkcyan;

  font-family: 'Montserrat', arial, sans-serif;
  font-size: 1rem;
  font-weight: normal;
}

h1 {
  color: #35495e;
}
</style>
