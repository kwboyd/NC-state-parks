<template>
  <div :addedParks="addedParks" :parks="parks" class="column">
    <!-- displays add/remove buttons depending on if the clicked park has been added -->
    <div id="park-name-box">
      <span v-if="currentParkIndex == 'preload'"></span>
      <button class="btn" @click="addPark(parks[currentParkIndex].number)" v-else-if="!parks[currentParkIndex].added">Add</button>
      <button class="btn" @click="removePark(parks[currentParkIndex].number)" v-else-if="parks[currentParkIndex].added">Remove</button>
    <!-- checks if currentParkIndex is set to the preload default values -->
      <h1 v-if="currentParkIndex == 'preload'"> Pick a park </h1>
      <h1 v-else> {{ parks[currentParkIndex].name }} </h1>
    </div>
      <h2 v-if="currentParkIndex == 'preload'"> Add a park </h2>
      <h2 v-else> {{ parks[currentParkIndex].description }} </h2>
  </div>
</template>
<script>
export default {
  props: ['parks', 'addedParks'],
  methods: {
    updateIndex: function (parksIndex) {
      // updates the index with the clicked park
      this.currentParkIndex = parksIndex
      console.log(this.currentParkIndex)
    },
    addPark: function (parkNumber) {
      // emits the parkAdded event and passes the index of the park
      this.$evt.$emit('parkAdded', parkNumber)
    },
    removePark: function (parkNumber) {
      // emits the parkRemoved event and passes the index of the park
      this.$evt.$emit('parkRemoved', parkNumber)
    }
  },
  mounted () {
    console.log('info -> mounted')
  // listens for a marker to be clicked, updates index
    this.$evt.$on('markerClicked', this.updateIndex)
  },
  beforeDestroy () {
    console.log('info -> beforeDestroy')
    this.$evt.$off('markerClicked', this.updateIndex)
  },
  data () {
    return {
      currentParkIndex: 'preload'
    }
  }
}
</script>
<style scoped>
#park-name-box {
  display: flex;
  align-items: center;
}
</style>
