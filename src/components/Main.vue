<script>
// Importo i componenti ApiPokemon e MyPokemon dalle rispettive directory
import ApiPokemon from './pages/ApiPokemon.vue';
import MyPokemon from './pages/MyPokemon.vue';

export default {
  data() {
    return {
      // Inizializzo un array vuoto per i Pokémon catturati
      caughtPokemons: [],
    };
  },
  methods: {
    // Carico i Pokémon catturati dalla memoria locale e li assegno a caughtPokemons
    loadCaughtPokemons() {
      const pokemons = JSON.parse(localStorage.getItem('caughtPokemons')) || [];
      this.caughtPokemons = pokemons;
    },
    // Aggiungo un Pokémon all'array caughtPokemons e aggiorno la memoria locale
    updateCaughtPokemons(pokemon) {
      this.caughtPokemons.push(pokemon);
      localStorage.setItem('caughtPokemons', JSON.stringify(this.caughtPokemons));
    },
    // Gestisco l'aggiornamento dei Pokémon catturati sostituendo l'array corrente con quello aggiornato
    handleUpdateCaughtPokemons(updatedPokemons) {
      this.caughtPokemons = updatedPokemons;
    },
  },
  // Registro i componenti ApiPokemon e MyPokemon per l'utilizzo all'interno del template
  components: {
    ApiPokemon,
    MyPokemon
  },
  // Chiamo la funzione loadCaughtPokemons quando il componente viene creato
  created() {
    this.loadCaughtPokemons();
  }
};
</script>

<template>
  <main>
    <div class="container-fluid">
      <div class="row shadow">
        <div class="col-6 l-box">
          <!-- Utilizzo il componente ApiPokemon e ascolto l'evento pokemon-caught per aggiornare i Pokémon catturati -->
          <ApiPokemon @pokemon-caught="updateCaughtPokemons" />
        </div>
        <div class="col-6 r-box">
          <!-- Utilizzo il componente MyPokemon, passando i Pokémon catturati e ascoltando l'evento update-caught-pokemons -->
          <MyPokemon :caughtPokemons="caughtPokemons" @update-caught-pokemons="handleUpdateCaughtPokemons" />
        </div>
      </div>
    </div>
  </main>
</template>

<style lang="scss" scoped>
main {
  height: 70vh;
  border: 20px solid rgb(149, 13, 13);
  border-radius: 10px;
  background-color: rgb(215, 11, 11);
  width: 900px;
}
.r-box, .l-box {
  height: 66vh;
}
.r-box {
  border-left: 5px solid rgb(149, 13, 13);
}
</style>
