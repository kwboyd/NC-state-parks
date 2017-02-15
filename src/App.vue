<template>
  <div id="app">
    <questions></questions>
    <mapgoogle :parks="parks"></mapgoogle>
    <info :parks="parks"></info>
    <parklist></parklist>
    <stats></stats>
  </div>
</template>

<script>
import axios from 'axios'
import mapgoogle from './components/mapgoogle'
import info from './components/info'
import parklist from './components/parklist'
import stats from './components/stats'
import questions from './components/questions'
export default {
  name: 'app',
  components: {
    mapgoogle,
    info,
    parklist,
    stats,
    questions
  },
  data () {
    return {
      parks: [
        {
          'name': '',
          'description': '',
          'coords': ''
        }
      ],
      currentParkIndex: ''
    }
  },
  methods: {
    getAxios () {
      // fetches park json
      axios.get('/static/parks.json')
        .then((response) => {
          console.log(response.data)
          this.parks = response.data
          console.log(this.parks)
          this.$evt.$emit('dataLoaded')
        })
    },
    setParkRemoved (currentParkIndex) {
      // marks the park at the currentParkIndex as not being added
      this.parks[currentParkIndex].added = false
    }
  },
  mounted () {
    console.log('app -> mounted')
    this.getAxios()
    // listens for a park to be removed, calls setParkRemoved
    this.$evt.$on('parkRemoved', this.setParkRemoved)
  },
  beforeDestroy () {
    this.$evt.$off('parkRemoved', this.setParkRemoved)
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
