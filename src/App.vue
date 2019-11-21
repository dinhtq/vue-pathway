<template>
  <div id="app">
    <PathFinder />
  </div>
</template>

<script>
import PathFinder from './components/PathFinder.vue'

export default {
  name: 'app',
  components: {
    PathFinder
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
  margin-top: 100px;
}
</style>
