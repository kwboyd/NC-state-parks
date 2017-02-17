<template>
  <div class="column is-one-third">
    <div id="stats-container">
      <p class="color-text" id="your-trip">Your trip:</p>
      <p class="stats-text"> Total distance: <span class="color-text">{{ distance }}</span> </p>
      <p class="stats-text"> Total driving time: <span class="color-text">{{ duration }}</span> </p>
    </div>
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
      this.miles = distance / 1609
      this.distance = this.miles.toFixed(0) + ' miles'
      // converts duration to hours and minutes
      // this code based partially on http://stackoverflow.com/questions/1322732/convert-seconds-to-hh-mm-ss-with-javascript
      var hours = Math.floor(duration / 3600)
      duration %= 3600
      var minutes = Math.floor(duration / 60)
      // checks if minute/hours both need to be displayed and if they need to be plural
      if (hours < 1 && minutes === 1) {
        this.duration = '1 minute'
      } else if (hours < 1) {
        this.duration = minutes + ' minutes'
      } else if (hours === 1 && minutes === 0) {
        this.duration = hours + ' hour'
      } else if (hours > 1 && minutes === 0) {
        this.duration = hours + ' hours'
      } else if (hours >= 1) {
        this.duration = hours + ' hours ' + minutes + ' minutes'
      }
    }
  },
  mounted () {
    console.log('stats -> mounted')
    this.$evt.$on('legsUpdated', this.updateStats)
  },
  beforeDestroy () {
    console.log('stats -> beforeDestroy')
    this.$evt.$off('legsUpdated', this.updateStats)
  }
}
</script>

<style>
.stats-text {
  color: #000;
  font-size: 1.25em;
}
#stats-container {
  padding: 6px;
  text-align: left;
}
#your-trip {
  font-size: 1.4em;
}
</style>
