<template>
  <div class="level">
    <div class="level-item">
      <p>Starting location</p>
      <input v-model="start" placeholder="City, State OR Zip" @input="updateStart">
    </div>
    <div class="level-item">
      <p>Ending location</p>
      <input v-model="end" placeholder="City, State OR Zip" @input="updateEnd">
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      end: 'Chapel Hill, NC',
      start: 'Chapel Hill, NC'
    }
  },
  methods: {
    updateStart () {
      // emits an event that the starting location has been updated
      this.$evt.$emit('startUpdated', this.start)
    },
    updateEnd () {
      // emits an event that the ending location has been updated
      this.$evt.$emit('endUpdated', this.end)
    }
  }
}
</script>

<style>
</style>
