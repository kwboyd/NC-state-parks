<template>
  <div id="app">
    <img src="./assets/logo.png">
    <mapgoogle></mapgoogle>
  </div>
</template>

<script>
var googlescript = document.createElement('script')
googlescript.setAttribute('type', 'text/javascript')
googlescript.async = true
googlescript.defer = true
googlescript.setAttribute('src', 'https://maps.googleapis.com/maps/api/js?key=AIzaSyAvSWIBEUD5mSVcV5U1Ev51tkMp0ugWJk8')
document.getElementsByTagName('head')[0].appendChild(googlescript)
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
