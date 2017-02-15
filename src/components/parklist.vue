<template>
  <div :addedParks="addedParks">
    <!-- creates list of added parks -->
    <div v-for="park in addedParks">
      <div v-show="addedParks.length > 0">
        <p>{{ park.name }}<p>
        <button class="btn" @click="removeParkFromList(park.number)">Remove</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  // data () {
  //   return {
  //     // sets default that the parkList is empty
  //     parkList: false
  //   }
  // },
  props: ['addedParks'],
  methods: {
    // updateParkList () {
    //   // adds the list of added parks to the parkList
    //   this.parkList = addedParks
    // },
    removeParkFromList (parkNumber) {
      // emits an event saying the park was removed, passes index of park
      this.$evt.$emit('parkRemoved', parkNumber)
    }
  },
  mounted () {
    // checks if the waypt array was changed, updates the parkList
  //  this.$evt.$on('wayptAdded', this.updateParkList)
  //  this.$evt.$on('wayptRemoved', this.updateParkList)
  },
  beforeDestroy () {
  //  this.$evt.$off('wayptAdded', this.updateParkList)
  //  this.$evt.$off('wayptRemoved', this.updateParkList)
  }
}
</script>

<style>
</style>
