<template>
  <div>
    <p>hello</p>
    <p> {{ distance }} </p>
    <p> {{ duration }} </p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      distance: '',
      duration: ''
    }
  },
  methods: {
    updateStats (distance, duration) {
      // converts distance from meters to miles
      var miles = distance / 1609
      this.distance = miles.toFixed(0) + ' miles'
      var hours = Math.floor(duration / 3600)
      duration %= 3600
      var minutes = Math.floor(duration / 60)
      if (hours < 1 && minutes === 1) {
        this.duration = '1 minutes'
      } else if (hours < 1) {
        this.duration = minutes + ' minutes'
      } else if (hours === 1 && minutes === 0) {
        this.duration = hours + ' hours'
      } else if (hours > 1 && minutes === 0) {
        this.duration = hours + ' hours'
      } else if (hours >= 1) {
        this.duration = hours + ' hours ' + minutes + ' minutes'
      }
    }
  },
  mounted () {
    this.$evt.$on('legsUpdated', this.updateStats)
  },
  beforeDestroy () {
    this.$evt.$off('legsUpdated', this.updateStats)
  }
}
</script>

<style>
</style>
