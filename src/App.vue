<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Celender/>
  </div>
</template>

<script>
import Celender from './components/Celender.vue';

export default {
  name: 'App',
  components: {
    Celender
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
