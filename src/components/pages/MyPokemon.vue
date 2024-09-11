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
      <ul class="p-0">
        <li class="list-unstyled bg-white mt-2">
          <div class="d-flex justify-content-between align-items-center p-2">
            <div class="d-flex align-items-center">
              <img src="" alt="poke" style="width: 50px; height: 50px; object-fit: cover; margin-right: 10px;" />
              <h5 class="m-0">nome</h5>
            </div>
            <div class="d-flex justify-content-end">
              <button class="btn btn-primary me-2"><Modal /></button>
              <button class="btn btn-secondary">e</button>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</div>

</template>

<style lang="scss" scoped></style>
