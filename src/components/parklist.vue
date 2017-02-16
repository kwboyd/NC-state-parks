<template>
  <div class="panel" :addedParks="addedParks">
    <!-- creates list of added parks -->
    <div class="panel-block" v-for="park in addedParks">
      <div v-show="addedParks.length > 0">
        <p>{{ park.name }}<p>
        <button class="btn" @click="removeParkFromList(park.number)">Remove</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['addedParks'],
  methods: {
    removeParkFromList (parkNumber) {
      // emits an event saying the park was removed, passes index of park
      this.$evt.$emit('parkRemoved', parkNumber)
    }
  }
}
</script>

<style>
</style>
