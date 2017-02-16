<template>
  <div class="column is-two-thirds":addedParks="addedParks">
    <!-- creates list of added parks -->
    <div class="list-items" v-for="park in addedParks">
      <div class="list-item" v-show="addedParks.length > 0">
        <button class="btn" @click="removeParkFromList(park.number)">Remove</button>
        <p>{{ park.name }}<p>
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
  .list-items {
    display: flex;
    flex-wrap: wrap;
  }
  .list-item {
    flex: 1 0 25%;
    box-sizing: border-box;
    display: flex;
    align-items: center;
  }
</style>
