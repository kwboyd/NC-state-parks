<template>
  <div>
  <b>Start:</b>
  <input v-model="start" placeholder="City, State OR Zip" @input="updateStart">
  <br>
  <b>End:</b>
  <input v-model="end" placeholder="City, State OR Zip" @input="updateEnd">
  <br>
  <b>Your car's miles per gallon:</b>
  <input v-model="mpg" type="number" @input="updateMpg">
  </div>
</template>

<script>
export default {
  data () {
    return {
      mpg: 25,
      end: 'Chapel Hill, NC',
      start: 'Chapel Hill, NC'
    }
  },
  methods: {
    updateStart () {
      this.$evt.$emit('startUpdated', this.start)
    },
    updateEnd () {
      this.$evt.$emit('endUpdated', this.end)
    },
    updateMpg () {
      this.$evt.$emit('mpgUpdated', this.mpg)
    }
  }
}
</script>

<style>
</style>
