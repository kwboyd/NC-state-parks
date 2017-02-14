<template>
  <div :parks="parks" id="container">
    <!-- checks if currentParkIndex is set to the preload default values -->
      <h1 v-if="currentParkIndex == 'preload'"> Pick a park </h1>
      <h1 v-else> {{ parks[currentParkIndex].name }} </h1>
      <h2 v-if="currentParkIndex == 'preload'"> Add a park </h2>
      <h2 v-else> {{ parks[currentParkIndex].description }} </h2>
      <!-- displays add/remove buttons depending on if the clicked park has been added -->
      <span v-if="currentParkIndex == 'preload'"></span>
      <button class="btn" @click="addPark" v-else-if="!parks[currentParkIndex].added">Add</button>
      <button class="btn" @click="removePark" v-else-if="parks[currentParkIndex].added">Remove</button>
  </div>
</template>
<script>
export default {
  props: {
    parks: {
      //defaults for before parks json is loaded
      default: [{
        "name": 'Pick a park',
        "description": 'Click a park',
        "coords": ''
        }]
    }
},
  methods: {
    updateIndex: function(parksIndex) {
      //updates the index with the clicked park
      this.currentParkIndex = parksIndex
      console.log(this.currentParkIndex)
    },
    addPark: function () {
      //emits the parkAdded event and passes the index of the park
      this.$evt.$emit('parkAdded', this.currentParkIndex)
      this.parks[this.currentParkIndex].added = true
    },
    removePark: function() {
      //emits the parkRemoved event and passes the index of the park
      this.$evt.$emit('parkRemoved', this.currentParkIndex)
      this.parks[this.currentParkIndex].added = false
    }
  },
  mounted() {
  console.log('info -> mounted')
  //listens for a marker to be clicked, updates index
  this.$evt.$on('markerClicked', this.updateIndex)
  },
  beforeDestroy() {
    console.log('info -> beforeDestroy')
    this.$evt.$off('markerClicked', this.updateIndex)
  },
  data () {
    return {
      currentParkIndex: 'preload',
    }
  }
}
</script>
<style scoped>
 #container {
  position: absolute;
   top: 500px;
 }
 .btn {
   padding: 10px;
   background-color: teal;
   color: white;
   float: left;
   margin-left: 30px;
 }
</style>
