<template>
  <div id="info-container" class="column" :addedParks="addedParks" :parks="parks">
    <!-- displays add/remove buttons depending on if the clicked park has been added, also checks if the currentParkIndex is at preload-->
    <div id="park-name-box">
      <span v-if="currentParkIndex == 'preload'"></span>
      <!-- passes the number property of the displayed park, which matches the index of the park in the parks array -->
      <button class="btn" @click="addPark(parks[currentParkIndex].number)" v-else-if="!parks[currentParkIndex].added">Add</button>
      <button class="btn" @click="removePark(parks[currentParkIndex].number)" v-else-if="parks[currentParkIndex].added">Remove</button>
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
    },
    addPark: function (parkNumber) {
      // emits the parkAdded event and passes the number property of the park
      this.$evt.$emit('parkAdded', parkNumber)
    },
    removePark: function (parkNumber) {
      // emits the parkRemoved event and passes the number property of the park
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
<style>
#park-name-box {
  display: flex;
  align-items: center;
}
#info-container {
  max-height: 275px;
  overflow: scroll;
  margin-right: 25px;
}
</style>
