<template>
  <div class="container">
    <h1>Pokedex</h1>
    <hr>
    <p class="d-block d-lg-none text-secondary">Clique na imagem para ver o pokemon de costas</p>
    <p class="d-none d-lg-block text-secondary">Passe o mouse para ver o pokemon de costas</p>
    <div class="input-group mb-3">
      <input type="search" name="busca" id="busca" class="form-control" placeholder="Buscar pokemon" v-model="busca">
      <button class="btn btn-primary" type="button" id="bt-buscar" @click="buscar()">Buscar</button>
      <button class="btn btn-danger" type="button" @click="limpar()">Limpar</button>
    </div>
    
    <div class="row">
      <div class="col-xl-2 col-lg-4 col-md-6" v-for="(pokemon, index) in filteredPokemons" :key="pokemon.url">
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
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function () {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=720&offset=0").then(response => {
      this.pokemons = response.data.results,
      this.filteredPokemons = response.data.results
    });
  },
  methods: {
    buscar: function() {
      if (this.busca.trim() == '') {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.search(this.busca.trim().toLowerCase()) >= 0);
      }
    },
    limpar: function() {
      this.busca = '';
      this.filteredPokemons = this.pokemons;
    }
  },
  computed: {
    resultadoBusca: function() {
      if (this.busca.trim() == '') {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
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
