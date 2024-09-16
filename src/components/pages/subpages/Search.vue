<script>
// Esporto il componente come oggetto JavaScript
export default {
  // Definisco le props che il componente riceve
  props: {
    suggestions: {
      type: Array, // suggestions è un array
      default: () => [] // Valore di default per suggestions è un array vuoto
    }
  },
  data() {
    return {
      query: '', // Inizializzo query come stringa vuota
      isOpen: false, // isOpen è inizialmente falso, usato per mostrare/nascondere i suggerimenti
    };
  },
  methods: {
    // Metodo chiamato al submit del form di ricerca
    onSearch(event) {
      event.preventDefault(); // Previene il comportamento di default del form
      this.$emit('search', this.query); // Emetto un evento 'search' con la query come payload
    },
    // Metodo chiamato ad ogni input nel campo di ricerca
    onInput() {
      this.$emit('search', this.query); // Emetto un evento 'search' con la query come payload
      this.controll(); // Controllo se aprire la lista dei suggerimenti
    },
    // Metodo chiamato quando si seleziona un suggerimento
    selectSuggestion(suggestion) {
      this.query = suggestion.name; // Imposto la query al nome del suggerimento selezionato
      this.isOpen = false; // Chiudo la lista dei suggerimenti
      this.$emit('select-suggestion', suggestion.name); // Emetto un evento 'select-suggestion' con il nome del suggerimento
    },
    // Metodo per aprire la lista dei suggerimenti
    controll() {
      this.isOpen = true; // Imposto isOpen a true per mostrare la lista dei suggerimenti
    }
  },

};
</script>
<template>
  <!-- Container principale -->
  <div class="search-container">
    <!-- Form per la ricerca -->
    <form class="d-flex mt-2" role="search" @submit="onSearch">
      <!-- Campo di input per la ricerca -->
      <input 
        v-model="query"  
        @input="onInput"
        class="form-control me-2 " 
        type="search" 
        placeholder="Search..." 
        aria-label="Search"
      ><!-- Associo il valore del campo di input alla variabile query -->
       <!-- Ascolto l'evento input e chiamo il metodo onInput -->
    </form>
    <!-- Lista dei suggerimenti, mostrata solo se isOpen è true -->
    <ul v-if="isOpen" class="list-group mt-2 suggestion-list">
      <!-- Ciclo attraverso le suggestions filtrate in base alla query -->
      <li 
        v-for="suggestion in suggestions.filter(pokemon => pokemon.name.toLowerCase().includes(query.toLowerCase()))" 
        :key="suggestion.name" 
        class="list-group-item list-group-item-action "
        @click="selectSuggestion(suggestion)" 
      ><!-- Chiamo selectSuggestion quando si clicca su un suggerimento -->
        {{ suggestion.name }} <!-- Mostro il nome del suggerimento -->
      </li>
    </ul>
  </div>
</template>

<style scoped>
.g{
  background-color: #9FB5A2;
}
.search-container {
  position: relative; 
}

.suggestion-list {

  max-height: 200px;
  overflow-y: auto; 
  position: absolute; 
  top: 100%;
  left: 0;
  width: 100%;
  z-index: 1000; 
  li{
    border: 1px solid black;
    color: #000000;
    background-color: #ffffff;
  }
}
</style>


