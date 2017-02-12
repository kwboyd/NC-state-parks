<template>
  <div id="app">
    <mapgoogle></mapgoogle>
  </div>
</template>

<script>
import axios from 'axios'
import mapgoogle from './components/mapgoogle'
export default {
  name: 'app',
  components: {
    mapgoogle
  },
  data () {
    return {
      parks: []
    }
  },
  mounted() {
    axios.get('/static/parks.json')
      .then((response) => {
        console.log(response.data)
        this.parks = response.data
        console.log(this.parks[0].name)
    })
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
