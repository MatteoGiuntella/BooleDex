<script>
import Modal from "./subpages/Modal.vue";

export default {
  props: {
    caughtPokemons: {
      type: Array,
      required: true,
    },
  },
  components: {
    Modal,
  },
};
</script>

<template>
  <div class="overflow-y-auto">
    <div class="row">
      <div class="col-12">
        <ul class="p-0">
          <li v-for="pokemon in caughtPokemons" :key="pokemon.name" class="list-unstyled bg-white mt-2">
            <div class="d-flex justify-content-between align-items-center p-2">
              <div class="d-flex align-items-center">
                <img
                  :src="pokemon.sprites.front_default"
                  alt="poke"
                  style="width: 50px; height: 50px; object-fit: cover; margin-right: 10px;"
                />
                <h5 class="m-0">{{ pokemon.name }}</h5>
              </div>
              <div class="d-flex justify-content-end">
                <button class="btn me-2">
                  <Modal />
                </button>
                <button class="btn">
                  <div class="box-button">
                    <img
                      src="/public/img/Progetto senza titolo (3).png"
                      alt=""
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
