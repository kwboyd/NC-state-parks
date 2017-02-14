<template>
  <div>
    <div v-for="l in parkList">
      <p v-show="parkList">{{ l.location }}<p>
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
    updateParkList (waypts) {
      this.parkList = waypts
      console.log(this.parkList[0].location)
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
