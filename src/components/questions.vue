<template>
  <div class="level">
    <div class="level-item">
      <p>Starting location</p>
      <input v-model="startPlace" placeholder="City, State OR Zip" @input="updateStart">
    </div>
    <div class="level-item">
      <p>Ending location</p>
      <input v-model="endPlace" placeholder="City, State OR Zip" @input="updateEnd">
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      endPlace: 'Chapel Hill, NC',
      startPlace: 'Chapel Hill, NC'
    }
  },
  methods: {
    updateStart () {
      // emits an event that the starting location has been updated
      this.$evt.$emit('startUpdated', this.startPlace)
    },
    updateEnd () {
      // emits an event that the ending location has been updated
      this.$evt.$emit('endUpdated', this.endPlace)
    }
  }
}
</script>

<style>
</style>
