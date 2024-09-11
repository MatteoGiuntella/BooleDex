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
      try {
        const response = await axios.get(this.pokemons[this.counter].url);
        this.singlePokemon = response.data;
        console.log(this.singlePokemon);
      } catch (error) {
        console.log(error);
      }
    },
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
    <div class="card bg-dark mt-2">
      <div v-if="singlePokemon">
        <div class="box-img mx-auto">
          <img
            :src="singlePokemon.sprites.front_default"
            class="card-img-top"
            alt="..."
          />
        </div>
        <div class="card-body">
          <h5 class="card-title text-white">{{ singlePokemon.name }}</h5>
          <p class="card-text text-white">
            Tipo : {{ singlePokemon.types[0].type.name }}
          </p>
        </div>
        <ul class="list-group list-group-flush">
          <li v-for="elem in singlePokemon.stats" class="list-group-item">
        
            <div class="">
                <p class=" text-start">{{ elem.stat.name }}</p>
              <div
                class="progress d-flex flex-grow-1"
                role="progressbar"
                aria-label="Basic example"
                :aria-valuenow="elem.base_stat"
                aria-valuemin="0"
                aria-valuemax="100"
              >
                <div
                  class="progress-bar"
                  :style="{ width: elem.base_stat + '%' }"
                ></div>
              </div>
            </div>
          </li>
        </ul>
      </div>

      <!-- <div class="card-body">
        <button type="button" class="btn btn-light mt-3">Light</button>
      </div> -->
    </div>
  </div>
</template>

<style lang="scss" scoped>
.card{
    padding: 0 !important;
}
.box-img {
  height: 150px;
  width: 150px;
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
  }
}
</style>
