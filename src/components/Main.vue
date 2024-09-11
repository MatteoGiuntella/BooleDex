<script>
import ApiPokemon from './pages/ApiPokemon.vue';
import MyPokemon from './pages/MyPokemon.vue';

export default {
    data() {
        return {

        };
    },
    methods: {

    },
    components:
  {
    ApiPokemon,
    MyPokemon
  }
}
</script>

<template>
   <main>
        <div class="container-fluid">
            <div class="row">
                <div class="col-6 l-box">
                    <ApiPokemon />
                </div>
                <div class="col-6 r-box">
                    <MyPokemon />
                </div>
            </div>
        </div>
   </main>
</template>

<style lang="scss" scoped>
main{
   height: 70vh;
   border: 20px solid rgb(149, 13, 13);
   border-radius: 10px;
   background-color: rgb(215, 11, 11);
    width: 900px;
 }
 .r-box, .l-box{
    height: 66vh;
 }
 .r-box{
    border-left: 5px solid rgb(149, 13, 13);
 }
</style>