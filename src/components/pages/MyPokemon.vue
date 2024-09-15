<script>

// Esporto il componente come oggetto JavaScript
export default {
  // Definisco le props che il componente riceve
  props: {
    caughtPokemons: {
      type: Array, // caughtPokemons è un array
      required: true, // È obbligatorio passare questa prop
    },
  },

  methods: {
    // Metodo per rimuovere un Pokémon dalla lista dei Pokémon catturati
    removePokemon(pokemonName) {
      // Filtra i Pokémon per escludere quello con il nome passato
      const updatedPokemons = this.caughtPokemons.filter(pokemon => pokemon.name !== pokemonName);  
      // Aggiorno la localStorage con la lista aggiornata dei Pokémon catturati
      localStorage.setItem('caughtPokemons', JSON.stringify(updatedPokemons));
      // Emetto un evento per aggiornare la lista dei Pokémon catturati nel componente genitore
      this.$emit('update-caught-pokemons', updatedPokemons);
    }
  }
};
</script>

<template>
  <!-- Container per la lista dei Pokémon catturati -->
  <div class="scroll">
    <div class="row g-0">
      <div class="col-12">
        <ul class="p-0">
          <!-- Ciclo attraverso caughtPokemons per creare una lista di Pokémon -->
          <li v-for="pokemon in caughtPokemons" :key="pokemon.name" class="list-unstyled bg-white mt-2 border-1 rounded-3">
            <div class="d-flex justify-content-between align-items-center p-2">
              <div class="d-flex align-items-center">
                <!-- Mostro l'immagine e il nome del Pokémon -->
                <img
                  :src="pokemon.sprites.front_default"
                  alt="poke"
                  style="width: 80px; height: 80px; object-fit: cover; margin-right: 10px;"
                />
                <h5 class="m-0">{{ pokemon.name }}</h5>
              </div>
              <div class="">
                <!-- Bottone per rimuovere il Pokémon -->
                <button class="btn" @click="removePokemon(pokemon.name)">
                  <div class="box-button">
                    <img
                      src="/public/img/Progetto senza titolo (3).png"
                      alt="remove"
                    />
                  </div>
                </button>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
/* Stile per il contenitore con scrollbar */
.scroll {
  max-height: 98%; 
  overflow-y: auto;  
}

/* Stile per il bottone che contiene l'immagine per rimuovere il Pokémon */
.box-button {
  height: 50px;
  width: 50px;
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    position: center;
  }
}
</style>
