<script>
import Modal from "./subpages/Modal.vue";
export default {
  data() {
    return {};
  },
  methods: {},
  components: {
    Modal,
  },
};
</script>

<template>
  <div class="overflow-y-auto">
    <div class="row">
      <div class="col-12">
        <ul class="bg-white mt-3">
          <li class="list-unstyled">
            <div class="d-flex justify-content-between">
              <div class="d-flex align-items-center">
                <img src="" alt="poke" />
                <h5>nome</h5>
              </div>
              <div class="d-flex">
                <button><Modal /></button>
                <button>e</button>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
