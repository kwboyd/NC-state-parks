<template>
  <div id="app">
    <div class="container">
      <instructions></instructions>
      <questions></questions>
    <div class="columns">
      <mapgoogle :addedParks="addedParks" :parks="parks"></mapgoogle>
      <info :addedParks="addedParks" :parks="parks"></info>
    </div>
    <div class="columns is-gapless" id="your-trip-container">
      <stats></stats>
      <parklist :addedParks="addedParks"></parklist>
    </div>
    </div>
    <foot></foot>
    <credits :parks="parks"></credits>
  </div>
</template>

<script>
import axios from 'axios'
import mapgoogle from './components/mapgoogle'
import info from './components/info'
import parklist from './components/parklist'
import stats from './components/stats'
import questions from './components/questions'
import instructions from './components/instructions'
import credits from './components/credits'
import foot from './components/foot'
export default {
  name: 'app',
  components: {
    mapgoogle,
    info,
    parklist,
    stats,
    questions,
    instructions,
    credits,
    foot
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
          // sets the parks as the data
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
  background-image: linear-gradient(to top, #B7F8DB 0%, #50A7C2 100%);;
}
.column {
  margin: 10px 20px;
}
.color-text {
  color: #44a4bb;
  font-weight: bold;
}
#your-trip-container {
  background-color: #fff;
  border-radius: 8px;
  margin: 10px;
  padding: 10px 12px;
}
.colored-button {
  margin-right: 13px;
  color: #fff;
  background-color: #73c8ca;
  /*this styling from bulma*/
  padding-left: .75em;
  padding-right: .75em;
  cursor: pointer;
  height: 2.285em;
  font-size: 1rem;
  border-radius: 3px;
}

</style>
