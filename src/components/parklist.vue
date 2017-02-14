<template>
  <div>
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
      parkList: false
    }
  },
  methods: {
    updateParkList (addedParks) {
      this.parkList = addedParks
    },
    removeParkFromList (currentParkIndex) {
      this.$evt.$emit('parkRemoved', currentParkIndex)
    }
  },
  mounted () {
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
