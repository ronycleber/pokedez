<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <div v-for="(poke,index) in pokemonz" :key="index">
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
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Pegando Pokemonz");
        this.pokemonz = res.data.results;
        console.log(this.pokemonz);
      });
  },
  components: {
    Pokemon,
  },
  filters: {
    nameUpper: function (value) {
      return value[0].toUpperCase() + value.slice(1);
    },
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
</style>
