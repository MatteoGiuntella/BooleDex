<script>
import Search from "./subpages/Search.vue";
export default {
  data() {
    return {};
  },
  methods: {},
  components: {
    Search,
  },
};
</script>

<template>
  <div class="container">
    <Search />
    <div class="card bg-dark mt-4">
      <img src="..." class="card-img-top" alt="..." />
      <div class="card-body">
        <h5 class="card-title text-white">Nome Pokemon</h5>
        <p class="card-text text-white">piccola descrizione?</p>
      </div>
      <ul class="list-group list-group-flush">
        <li class="list-group-item">
          <span
            class="mt-2"><h5>stats</h5>
            <div
              class="progress"
              role="progressbar"
              aria-label="Basic example"
              aria-valuenow="25"
              aria-valuemin="0"
              aria-valuemax="100"
            >
              <div class="progress-bar" style="width: 25%"></div></div
          ></span>
        </li>
       
      </ul>
      <div class="card-body">
        <button type="button" class="btn btn-light mt-3">Light</button>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
