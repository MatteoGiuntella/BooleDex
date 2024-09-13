<script>
export default {
  props: {
    suggestions: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      query: '',
      isOpen : false,
    };
  },
  methods: {
    onSearch(event) {
      event.preventDefault();
      this.$emit('search', this.query);
    },
    onInput() {
      this.$emit('search', this.query);
      this.controll();
    },
    selectSuggestion(suggestion) {
      this.query = suggestion.name;
      this.isOpen = false,
      this.$emit('select-suggestion', suggestion.name);
    },
    controll(){
        this.isOpen = true
    }
  },

};
</script>

<template>
  <div>
    <form class="d-flex mt-2" role="search" @submit="onSearch">
      <input 
        v-model="query"
        @input="onInput"
        class="form-control me-2" 
        type="search" 
        placeholder="Search..." 
        aria-label="Search"
      >
    </form>
    <ul v-if="isOpen" class="list-group mt-2">
      <li 
        v-for="suggestion in suggestions.filter(pokemon => pokemon.name.toLowerCase().includes(query.toLowerCase()))" 
        :key="suggestion.name" 
        class="list-group-item list-group-item-action"
        @click="selectSuggestion(suggestion)"
      >
        {{ suggestion.name }}
      </li>
    </ul>
  </div>
</template>

<style lang="scss" scoped>

</style>
