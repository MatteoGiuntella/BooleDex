<script>
// Importo il componente Search e la libreria axios
import Search from "./subpages/Search.vue";
import axios from "axios";

export default {
  data() {
    return {
      // Inizializzo le variabili di stato per il componente
      pokemons: [],
      singlePokemon: null,
      counter: 0,
      searchQuery: '',
      flag: false,
      intervalId: null,
      showModal: false,
    };
  },
  methods: {
    // Recupero la lista dei Pokémon dall'API e la memorizzo in pokemons
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
    // Recupero i dettagli di un singolo Pokémon in base al contatore corrente
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
    // Gestisco la ricerca di un Pokémon in base alla query inserita
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
        console.log("Pokémon non trovato");
      }
    },
    // Seleziono un suggerimento di ricerca e recupero i dettagli del Pokémon corrispondente
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
    // Passo al Pokémon successivo incrementando il contatore
    nextPokemon() {
      this.counter++;
      this.GetSinglePokemon();
    },
    // Passo al Pokémon precedente decrementando il contatore
    previousPokemon() {
      this.counter--;
      this.GetSinglePokemon();
    },
    // Catturo il Pokémon corrente e lo memorizzo nella localStorage
    catchPokemon() {
      if (this.singlePokemon) {
        const caughtPokemons = JSON.parse(localStorage.getItem('caughtPokemons')) || [];
        caughtPokemons.push(this.singlePokemon);
        localStorage.setItem('caughtPokemons', JSON.stringify(caughtPokemons));
        this.$emit('pokemon-caught', this.singlePokemon); 
        this.showModal = true;
      }
    },
    // Chiudo il modal
    closeModal() {
      this.showModal = false;
    }
  },
  components: {
    Search,
  },
  // Carico i Pokémon quando il componente viene creato
  created() {
    this.GetPokemons();
  },
  // Avvio un intervallo per alternare tra le immagini del Pokémon quando il componente è montato
  mounted() {
    this.intervalId = setInterval(() => {
      this.flag = !this.flag;
    }, 3000);
  },
  computed: {
    // Cambio immagine del Pokémon tra fronte e retro ogni 3 secondi
    currentImg() {
      if (this.singlePokemon) {
        return this.flag ? this.singlePokemon.sprites.back_default : this.singlePokemon.sprites.front_default;
      }
      return '';
    }
  },
  // Pulisco l'intervallo prima di distruggere il componente
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
    <div class="bg mt-3 border-1 rounded-3">
      <div v-if="singlePokemon">
        <div class="box-img mx-auto">
          <img :src="currentImg" class="" alt="..." />
        </div>
        <div class="">
          <h4 class="">{{ singlePokemon.name }}</h4>
          <h5 class="">{{ singlePokemon.types[0].type.name }}</h5>
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

    <!-- Modal -->
    <div v-if="showModal" class="modal shadow-lg" tabindex="-1" style="display: block;" role="dialog">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header d-flex justify-content-between close">
            <h5 class="modal-title">Pokémon Catturato!</h5>
            <button type="button" class="close rounded-3" @click="closeModal">
              <span class=" fw-bold">&times;</span>
            </button>
          </div>
          <div class="modal-body close">
            <div class="box-img-2 mx-auto">
              <img :src="singlePokemon.sprites.other.showdown.front_default" class=" " alt="..." />
            </div>
            <div class="">
              <h4 class="">{{ singlePokemon.name }}</h4>
              <h5 class="">{{ singlePokemon.types[0].type.name }}</h5>
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
          </div>
          <!-- <div class="modal-footer">
            <button type="button" class="btn btn-secondary" @click="closeModal">Chiudi</button>
          </div> -->
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.bg, li, .close {
  background-color: #9FB5A2;
  border: 1px solid black;
}

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

.modal-backdrop {
  display: none;
}
.box-img-2{
  height: 200px;
  width: 250px;
  img {
    width: 100%;
    height: 100%;
    object-fit: contain;
    object-position: center;
  }
}
</style>
