<script setup>
import Header from './components/Header.vue';
import Main from './components/Main.vue';
import { createApp } from 'vue'

const app = createApp({
  data() {
    return {
      pokemon :[],
    }
  },
  methods :
  {

  },
  components:
  {
    Header,
    Main
  }
})
</script>

<template>

<Header></Header>,
<Main></Main>


</template>

<style scoped>

</style>
