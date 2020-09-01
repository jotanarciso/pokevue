<template>
  <div id="app">
    <header>
      <img src="./assets/logo.png" />
    </header>
    <main>
      <button class="btn-nav left" @click="previous">PREVIOUS</button>
      <button class="btn-nav right" @click="next">NEXT</button>
      <div class="cards-container">
        <card v-if="pokemonIndex" :index.sync="pokemonIndex"></card>
      </div>
    </main>
  </div>
</template>

<script>
import Card from "./components/Card";
import axios from "axios";

export default {
  name: "app",
  components: {
    Card,
  },
  data() {
    return {
      pokemonIndex: null,
    };
  },

  created() {
    this.fetchPokemonList();
  },

  updated() {},
  methods: {
    fetchPokemonList(url = "http://pokeapi.co/api/v2/pokemon/?limit=3") {
      axios.get(url).then(({ data }) => (this.pokemonIndex = data));
    },
    next() {
      this.fetchPokemonList(this.pokemonIndex.next);
    },
    previous() {
      this.fetchPokemonList(this.pokemonIndex.previous);
    },
  },
};
</script>

<style>
body {
  margin: 0;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  background: url(./assets/background.png) no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

main {
  text-align: center;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px 20px 0;
}

header img {
  width: 200px;
}

header span {
  display: block;
  position: relative;
  font-size: 20px;
  line-height: 1;
  letter-spacing: 0.02em;
  font-weight: 400;
  box-sizing: border-box;
  padding-top: 16px;
}

.cards-container {
  min-height: 1500px;
}

.btn-nav {
  background: #f4cd30;
  color: #1a1a82;
  font-weight: bold;
  font-size: 25px;
  border-radius: 20px;
  cursor: pointer;
  top: 50vh;
  padding: 9px;
  border: 4px solid #312f99;
  position: fixed;
  z-index: 1;
}

.btn-nav.right {
  right: 10px;
}

.btn-nav.left {
  left: 10px;
}

.no-gutters {
  margin-right: 0;
  margin-left: 0;
}
</style>
