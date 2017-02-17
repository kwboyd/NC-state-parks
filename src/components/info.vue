<template>
  <div id="info-container" class="column" :addedParks="addedParks" :parks="parks">
    <!-- displays add/remove buttons depending on if the clicked park has been added, also checks if the currentParkIndex is at preload-->
    <span v-if="currentParkIndex == 'preload'"></span>
    <div v-else id="park-button-box">
      <!-- passes the number property of the displayed park, which matches the index of the park in the parks array -->
      <button class="colored-button" @click="addPark(parks[currentParkIndex].number)" v-if="!parks[currentParkIndex].added">Add to route</button>
      <button class="colored-button" @click="removePark(parks[currentParkIndex].number)" v-else="parks[currentParkIndex].added">Remove</button>
      <p>Scroll within this box to learn about this park.</p>
    </div>
    <div id="park-info-box" v-if="currentParkIndex == 'preload'">
      <p class="park-name"> Click on a map marker to pick a park</p>
      <p class="park-name"> An 'add to route' button will appear here, which will turn into a remove button if a park is already on your route.</p>
    </div>
    <div id="park-info-box" v-else>
      <p class="park-name"> {{ parks[currentParkIndex].name }} </p>
      <img :src="parks[currentParkIndex].image" :alt="parks[currentParkIndex].name">
      <p id="description"> {{ parks[currentParkIndex].description }} </p>
    </div>
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
#info-container {
  max-height: 302px;
  overflow: scroll;
  border-radius: 8px;
  background: #fff;
}
#park-button-box {
  display: flex;
  justify-content: center;
  align-items: center;
}
#park-button-box p {
  font-size: .85em;
}
.park-name {
  margin: 6px;
  padding-top: 3px;
  font-size: 1.5em;
}
#description {
  text-align: left;
  padding: 6px;
}
</style>
