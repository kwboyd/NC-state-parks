<template>
  <div id="app">
    <button class="btn" @click="emitUpdate">Update Map </button>
      <questions style="background-color:green;"></questions>
    <div class="columns">
      <mapgoogle :addedParks="addedParks" :parks="parks"></mapgoogle>
      <info style="background-color:pink;" :addedParks="addedParks" :parks="parks"></info>
    </div>
    <div class="columns">
      <stats style="background-color:gray;"></stats>
      <parklist :addedParks="addedParks" style="background-color:cyan;"></parklist>
    </div>
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
      addedParks: []
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
    setParkRemoved (parkIndex) {
      // marks the park at the currentParkIndex as not being added
      this.parks[parkIndex].added = false
      for (var t in this.addedParks) {
        if (this.parks[parkIndex].name === this.addedParks[t].name) {
          this.addedParks.splice(t, 1)
        }
      }
      this.$evt.$emit('parkRemoveComplete', parkIndex)
    },
    setParkAdded (parkIndex) {
      this.parks[parkIndex].added = true
      this.addedParks.push(this.parks[parkIndex])
      this.$evt.$emit('parkAddComplete', parkIndex)
    },
    emitUpdate () {
      this.$evt.$emit('updateClicked')
    }
  },
  mounted () {
    console.log('app -> mounted')
    this.getAxios()
    // listens for a park to be removed, calls setParkRemoved
    this.$evt.$on('parkRemoved', this.setParkRemoved)
    this.$evt.$on('parkAdded', this.setParkAdded)
  },
  beforeDestroy () {
    this.$evt.$off('parkRemoved', this.setParkRemoved)
    this.$evt.$off('parkAdded', this.setParkAdded)
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
.btn {
  padding: 10px;
  background-color: teal;
  color: white;
  margin-left: 30px;
}
</style>
