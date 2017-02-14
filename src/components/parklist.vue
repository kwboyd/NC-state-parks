<template>
  <div>
    <!-- creates list of added parks -->
    <div v-for="l in parkList">
      <div v-show="parkList">
        <p>{{ l.parkName }}<p>
        <button class="btn" @click="removeParkFromList(l.wayptParkIndex)">Remove</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // sets default that the parkList is empty
      parkList: false
    }
  },
  methods: {
    updateParkList (addedParks) {
      // adds the list of added parks to the parkList
      this.parkList = addedParks
    },
    removeParkFromList (currentParkIndex) {
      // emits an event saying the park was removed, passes index of park
      this.$evt.$emit('parkRemoved', currentParkIndex)
    }
  },
  mounted () {
    // checks if the waypt array was changed, updates the parkList
    this.$evt.$on('wayptAdded', this.updateParkList)
    this.$evt.$on('wayptRemoved', this.updateParkList)
  },
  beforeDestroy () {
    this.$evt.$off('wayptAdded', this.updateParkList)
    this.$evt.$off('wayptRemoved', this.updateParkList)
  }
}
</script>

<style>
</style>
