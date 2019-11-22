<template>
  <div id="app">
    <SelectorDificultad v-if="!dificultad" v-on:dificultad="dificultad = $event"/>
    <Juego v-if="dificultad" v-bind:dificultad="dificultad" v-on:volver="dificultad = null"/>
  </div>
</template>

<script>
import SelectorDificultad from "./components/SelectorDificultad";
import Juego from "./components/Juego";

export default {
  name: 'app',
  components: {
    SelectorDificultad,
    Juego
  },
  data: function () {
    return {
      dificultad: null
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
