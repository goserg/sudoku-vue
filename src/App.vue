<template>
  <div id="app">
    <board></board>
  </div>
</template>

<script>
import board from "./components/board";

export default {
  name: 'App',
  components: {
    board
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
