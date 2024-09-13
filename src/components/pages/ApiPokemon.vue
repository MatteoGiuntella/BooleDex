<script>
import Search from "./subpages/Search.vue";
import axios from "axios";

export default {
  data() {
    return {
      pokemons: [],
      singlePokemon: null,
      counter: 0,
      searchQuery: '',
      flag: false,
      intervalId: null, 
    };
  },
  methods: {
    async GetPokemons() {
      try {
        const response = await axios.get(
          `https://pokeapi.co/api/v2/pokemon?offset=200&limit=200`
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
    async handleSearch(query) {
      this.searchQuery = query;
      const pokemon = this.pokemons.find(pokemon =>
        pokemon.name.toLowerCase() === this.searchQuery.toLowerCase()
      );
      if (pokemon) {
        try {
          const response = await axios.get(pokemon.url);
          this.singlePokemon = response.data;
          console.log(this.singlePokemon);
        } catch (error) {
          console.log(error);
        }
      } else {
        // Gestione del caso in cui il Pokémon non è trovato
        console.log("Pokémon non trovato");
      }
    },
    async selectSuggestion(pokemonName) {
      this.searchQuery = pokemonName;
      const pokemon = this.pokemons.find(p => p.name.toLowerCase() === pokemonName.toLowerCase());
      if (pokemon) {
        try {
          const response = await axios.get(pokemon.url);
          this.singlePokemon = response.data;
          console.log(this.singlePokemon);
        } catch (error) {
          console.log(error);
        }
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
        this.$emit('pokemon-caught', this.singlePokemon); 
        alert(`${this.singlePokemon.name} è stato catturato!`);
      }
    },
  },
  components: {
    Search,
  },
  created() {
    this.GetPokemons();
  },
  mounted() {
    this.intervalId = setInterval(() => {
      this.flag = !this.flag;
    }, 3000);
  },
  computed: {
    currentImg() {
      if (this.singlePokemon) {
        return this.flag ? this.singlePokemon.sprites.back_default : this.singlePokemon.sprites.front_default;
      }
      return '';
    }
  },
  beforeDestroy() {
    clearInterval(this.intervalId);
  },
};
</script>

<template>
  <div class="container">
    <Search 
      :suggestions="pokemons"
      @search="handleSearch"
      @select-suggestion="selectSuggestion"
    />
    <div class="bg-white mt-3 border-1 rounded-3">
      <div v-if="singlePokemon">
        <div class="box-img mx-auto">
          <img :src="currentImg" class="" alt="..." />
        </div>
        <div class="">
          <h4 class="">{{ singlePokemon.name }}</h4>
          <h5 class="">Type : {{ singlePokemon.types[0].type.name }}</h5>
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
