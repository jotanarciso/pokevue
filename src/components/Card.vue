<template>
  <div class="container">
    <div class="row mb-2 mt-5">
      <div class="col-12" v-for="pokemon in pokemons">
        <div class="card fade-in">
          <div class="id fade-in">{{pokemon.id}}</div>
          <div class="xp fade-in">
            <span class="font-weight-bold fade-in">{{pokemon.base_experience}}</span>
            <span class="ml-1">xp</span>
          </div>
          <div class="d-flex justify-content-center sprite fade-in">
            <img class="fade-in" v-if="pokemon.sprites" :src="pokemon.sprites.front_default" alt />
            <div class="overlay"></div>
          </div>
          <div class="info">
            <h1 class="text-center font-weight-bold fade-in text-uppercase">{{ pokemon.name }}</h1>
            <div class="games">
              <p class="font-weight-bold mt-3 mb-1 text-center">Games</p>
              <span
                class="game fade-in"
                v-for="game in pokemon.game_indices"
                v-bind:key="pokemon.id"
              >{{game.version.name}} |</span>
            </div>
            <div class="cities">
              <p
                v-if="pokemon.cities.data.length > 0"
                class="font-weight-bold mt-3 mb-1 text-center"
              >Found on</p>
              <span
                class="game fade-in"
                v-for="city in pokemon.cities.data"
                v-bind:key="pokemon.id"
              >{{city.location_area.name}} |</span>
            </div>
          </div>
        </div>
        <!-- end card -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: {
    index: {
      type: Object,
    },
  },
  data() {
    return {
      pokemons: [],
    };
  },
  created() {
    this.fetchPokemons();
  },
  watch: {
    index: function () {
      this.fetchPokemons();
    },
  },
  methods: {
    fetchPokemons(url) {
      this.index.results.map((pokemon) => {
        axios.get(pokemon.url).then((result) => {
          this.mountWithCities(result.data);
        });
      });
    },

    mountWithCities(pokemon) {
      axios.get(pokemon.location_area_encounters).then((cities) => {
        let p = pokemon;
        p.cities = cities;
        this.pokemons.length < 3
          ? this.pokemons.push(p)
          : this.fetchPokemons((this.pokemons = []));
      });
    },
  },
};
</script>

<style>
/* Card  */
.card {
  height: 500px;
  width: 100%;
  position: relative;
  margin: 10px auto;
  background-size: 100%;
  background-repeat: no-repeat;
  background-position: center;
  background-color: #211799;
  background-size: cover;
  border: 2px solid #00000063;
  border-radius: 15px;
  padding: 15px;
  box-shadow: 3px 7px 3px 3px #00000026;
}

.card .id {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 0.9em;
  color: #f6f6f6;
  margin: 5px;
}

.card .sprite {
  height: 140px;
  border-radius: 15px;
  padding: 15px;
  position: relative;
  margin: 25px auto;
}

.card .sprite img {
  background: url("https://i.pinimg.com/originals/ca/e0/1a/cae01ab5cce960db0d7819cc96e97ce8.png");
  top: 0;
  z-index: 1;
  position: absolute;
  max-width: 150px;
  min-width: 150px;
  max-height: 150px;
  border: 2px solid #724b73;
  border-radius: 100px;
  box-shadow: 3px 7px 3px 3px #00000026;
}

.card .sprite .overlay {
  background: linear-gradient(90deg, #fff 0, #6898b9 54%, #ce0079 100%);
  opacity: 0.1;
  z-index: 2;
  border-radius: 100px;
  height: 150px;
  width: 150px;
  left: 75px;
  margin: -15px;
}

.card .info {
  height: 300px;
  width: 100%;
  margin: -5px auto;
  padding: 0;
  border-radius: 15px;
  color: #fff;
}

.card .info h1 {
  font-size: 2em;
  text-shadow: 3px 3px 3px #be2559;
}

.card .xp {
  color: #fff;
  display: flex;
  position: absolute;
  top: 10px;
  left: 10px;
  width: 25px;
  margin: 5px;
}

.card:before,
.card:after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  border-radius: 15px;
  background-image: linear-gradient(
    115deg,
    transparent 0%,
    #00e7ff 30%,
    #ff00e7 70%,
    transparent 100%
  );
  background-position: 0% 0%;
  background-repeat: no-repeat;
  background-size: 300% 300%;
  mix-blend-mode: color-dodge;
  opacity: 0.2;
  z-index: 1;
}

.card .game {
  margin: 5px;
  text-transform: uppercase;
}

.fade-in {
  opacity: 1;
  animation-name: fadeInOpacity;
  animation-iteration-count: 1;
  animation-timing-function: ease-in;
  animation-duration: 0.5s;
}

@keyframes fadeInOpacity {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
