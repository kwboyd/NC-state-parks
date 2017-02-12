<template>
  <div id="app">
    <mapgoogle :parks="parks"></mapgoogle>
    <info :parks="parks"></info>
  </div>
</template>

<script>
import axios from 'axios'
import mapgoogle from './components/mapgoogle'
import info from './components/info'
export default {
  name: 'app',
  components: {
    mapgoogle,
    info
  },
  data () {
    return {
      parks: []
    }
  },
  methods: {
    getAxios () {
      //fetches park json
      axios.get('/static/parks.json')
        .then((response) => {
        //  console.log(response.data)
          this.parks = response.data
        //  console.log(this.parks)
          this.$evt.$emit('dataLoaded')
      })
    },
    sayClicked () {
      console.log('hello')
    }
  },
  mounted() {
    console.log('app -> mounted')
    this.getAxios()
    this.$evt.$on('markerClicked', this.sayClicked())
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
