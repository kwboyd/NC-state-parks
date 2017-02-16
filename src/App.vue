<template>
  <div id="app">
    <div class="container">
      <button class="btn" @click="emitUpdate">Update Map </button>
      <questions></questions>
    <div class="columns">
      <mapgoogle :addedParks="addedParks" :parks="parks"></mapgoogle>
      <info :addedParks="addedParks" :parks="parks"></info>
    </div>
    <div class="columns">
      <stats style="background-color:gray;"></stats>
      <parklist :addedParks="addedParks" style="background-color:cyan;"></parklist>
    </div>
    <button class="button" @click="modalOpen">Credits</button>
    <credits></credits>
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
import credits from './components/credits'
export default {
  name: 'app',
  components: {
    mapgoogle,
    info,
    parklist,
    stats,
    questions,
    credits
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
          this.$evt.$emit('dataLoaded')
        })
    },
    setParkRemoved (parkIndex) {
      // marks the park at the parkIndex as not being added
      this.parks[parkIndex].added = false
      for (var i in this.addedParks) {
        // goes through addedParks and checks if the name matches the park at parkIndex's name
        if (this.parks[parkIndex].name === this.addedParks[i].name) {
          // if the names match, remove the park from the addedParks array
          this.addedParks.splice(i, 1)
        }
      }
      this.$evt.$emit('parkRemoveComplete')
    },
    setParkAdded (parkIndex) {
      // sets the park at parkIndex as added, then pushes it to the addedParks array
      this.parks[parkIndex].added = true
      this.addedParks.push(this.parks[parkIndex])
      this.$evt.$emit('parkAddComplete')
    },
    emitUpdate () {
      // emits an event to update the map
      this.$evt.$emit('updateClicked')
    },
    modalOpen () {
      this.$evt.$emit('openModal')
    }
  },
  mounted () {
    console.log('app -> mounted')
    this.getAxios()
    this.$evt.$on('parkRemoved', this.setParkRemoved)
    this.$evt.$on('parkAdded', this.setParkAdded)
  },
  beforeDestroy () {
    console.log('app -> beforeDestroy')
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
  background-image: linear-gradient(to top, #9be15d 0%, #00e3ae 100%);
}
.column {
  margin: 10px 20px;
}
</style>
