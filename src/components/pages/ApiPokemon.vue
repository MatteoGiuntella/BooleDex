<script>
import Search from "./subpages/Search.vue";
import axios from "axios";

export default {
  data() {
    return {
      pokemons: [],
      singlePokemon: null,
      counter: 0,
    };
  },
  methods: {
    async GetPokemons() {
      try {
        const response = await axios.get(
          "https://pokeapi.co/api/v2/pokemon?offset=200&limit=200"
        );
        this.pokemons = response.data.results;
        this.GetSinglePokemon();
        console.log(this.pokemons);
      } catch (error) {
        console.log(error);
      }
    },
    async GetSinglePokemon() {
      if (this.counter < 0) {
        this.counter = this.pokemons.length - 1;
      } else if (this.counter >= this.pokemons.length) {
        this.counter = 0;
      }

      try {
        const response = await axios.get(this.pokemons[this.counter].url);
        this.singlePokemon = response.data;
        console.log(this.singlePokemon);
      } catch (error) {
        console.log(error);
      }
    },
    nextPokemon() {
      this.counter++;
      this.GetSinglePokemon();
    },
    previousPokemon() {
      this.counter--;
      this.GetSinglePokemon();
    },
    catchPokemon() {
      if (this.singlePokemon) {
        const caughtPokemons = JSON.parse(localStorage.getItem('caughtPokemons')) || [];
        caughtPokemons.push(this.singlePokemon);
        localStorage.setItem('caughtPokemons', JSON.stringify(caughtPokemons));
        this.$emit('pokemon-caught', this.singlePokemon); // Emette l'evento con il Pokémon catturato
        alert(`${this.singlePokemon.name} è stato catturato!`);
      }
    }
  },
  components: {
    Search,
  },
  created() {
    this.GetPokemons();
  },
};
</script>

<template>
  <div class="container">
    <Search />
    <div class="bg-white mt-2">
      <div v-if="singlePokemon">
        <div class="box-img mx-auto">
          <img :src="singlePokemon.sprites.front_default" class="" alt="..." />
        </div>
        <div class="">
          <h5 class="">{{ singlePokemon.name }}</h5>
          <p class="">Tipo : {{ singlePokemon.types[0].type.name }}</p>
        </div>
        <ul class="list-group list-group-flush">
          <li
            v-for="elem in singlePokemon.stats"
            :key="elem.stat.name"
            class="list-group-item"
          >
            <div class="d-flex justify-content-between align-items-baseline">
              <div class="text-start me-3" style="min-width: 100px">
                {{ elem.stat.name }}
              </div>
              <div
                class="progress flex-grow-1"
                role="progressbar"
                aria-label="Success example 1px high"
                :aria-valuenow="elem.base_stat"
                aria-valuemin="0"
                aria-valuemax="100"
                style="height: 5px"
              >
                <div
                  class="progress-bar bg-success"
                  :style="{ width: elem.base_stat + '%' }"
                ></div>
              </div>
            </div>
          </li>
        </ul>
      </div>

      <div class="card-body">
        <button class="btn m-3" @click="previousPokemon">
          <i class="fa-solid fa-circle-left fa-2xl" style="color: #000000;"></i>
        </button>
        <button type="submit" class="btn my-3" @click="catchPokemon">
          <img
            src="/public/img/Poke_Ball.webp"
            width="40"
            height="34"
            alt="cattura"
            tool="cattura"
          />
        </button>
        <button class="btn m-3" @click="nextPokemon">
          <i class="fa-solid fa-circle-right fa-2xl" style="color: #000000;"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.box-img {
  height: 100px;
  width: 150px;
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
  }
}
</style>
