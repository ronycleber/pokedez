<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <h1 class="is-size-5">Pesquisar</h1>
      <input class="input is-rounded" type="text" placeholder="Digite um nome" v-model="busca">
      <button class="buscar button is-fullwidth is-success is-rounded" @click="buscar">Buscar</button>
      <div v-for="(poke,index) in pokemonzFiltrado" :key="poke.url">
        <Pokemon :name="poke.name | nameUpper" :url="poke.url" :num="index+1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  data() {
    return {
      pokemonz: [],
      pokemonzFiltrado: [],
      busca:''
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {        
        this.pokemonz = res.data.results;
        this.pokemonzFiltrado = res.data.results;
      })
  },
  components: {
    Pokemon,
  },
  filters: {
    nameUpper: function (value) {
      return value[0].toUpperCase() + value.slice(1);
    },
  },
  methods:{
    buscar: function(){      
      this.pokemonzFiltrado = this.pokemonz;
      if (this.busca == '' || this.busca ==' ') {
        this.pokemonzFiltrado = this.pokemonz;
      } else {        
        this.pokemonzFiltrado = this.pokemonzFiltrado.filter(pokemon => pokemon.name.match(this.busca.toLowerCase()));
      }
    }
  },
  computed:{
    // buscarPokemon: function(){
      // if (this.busca == '' || this.busca ==' ') {
        // return this.pokemonz;
      // } else {
        // return this.pokemonz.filter(pokemon => pokemon.name.match(this.busca) );
      // }
    // }
  }
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
.buscar {
  margin: 2% 0px;
}
</style>
