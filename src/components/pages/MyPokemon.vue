<script>


export default {
  props: {
    caughtPokemons: {
      type: Array,
      required: true,
    },
  },

  methods: {
    removePokemon(pokemonName) {
      const updatedPokemons = this.caughtPokemons.filter(pokemon => pokemon.name !== pokemonName);  
      localStorage.setItem('caughtPokemons', JSON.stringify(updatedPokemons));
      this.$emit('update-caught-pokemons', updatedPokemons);
    }
  }
};
</script>

<template>
  <div class="scroll">
    <div class="row g-0">
      <div class="col-12">
        <ul class="p-0">
          <li v-for="pokemon in caughtPokemons" :key="pokemon.name" class="list-unstyled bg-white mt-2 border-1 rounded-3">
            <div class="d-flex justify-content-between align-items-center p-2">
              <div class="d-flex align-items-center">
                <img
                  :src="pokemon.sprites.front_default"
                  alt="poke"
                  style="width: 80px; height: 80px; object-fit: cover; margin-right: 10px;"
                />
                <h5 class="m-0">{{ pokemon.name }}</h5>
              </div>
              <div class="">
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
.scroll {
  max-height: 98%; 
  overflow-y: auto;  
}

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
