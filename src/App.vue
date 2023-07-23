<template>
  <div class="container">
    <h1>Pokedex</h1>
    <hr>
    <div class="row">
      <div class="col-2" v-for="(pokemon, index) in pokemons" :key="index">
        <Pokemon :name="pokemon.name" :url="pokemon.url" :num="index + 1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon.vue';

export default {
  name: 'App',
  components: {
    Pokemon
  },
  data() {
    return {
      pokemons: []
    }
  },
  created: function () {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=180&offset=0").then(response => {
      this.pokemons = response.data.results
    });
  }
}
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
