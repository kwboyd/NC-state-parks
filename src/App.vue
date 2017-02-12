<template>
  <div id="app">
    <mapgoogle></mapgoogle>
  </div>
</template>

<script>
import mapgoogle from './components/mapgoogle'
export default {
  name: 'app',
  components: {
    mapgoogle
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
