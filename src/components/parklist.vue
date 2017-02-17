<template>
  <div class="column" :addedParks="addedParks">
    <!-- creates list of added parks -->
    <ul id="park-list-container">
      <!-- if the addedParks array isn't empty, display that park in the list -->
      <div class="list-item" v-for="park in addedParks" v-show="addedParks.length > 0">
        <!-- passes the number property of the clicked park, which equals the park's index in the parks array -->
        <button id="remove-button" class="colored-button" @click="removeParkFromList(park.number)">Remove</button>
        <p id="park-list-name">{{ park.name }}<p>
      </div>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['addedParks'],
  methods: {
    removeParkFromList (parkNumber) {
      // emits an event saying the park was removed, passes number property of park, which matches the park's index in the array
      this.$evt.$emit('parkRemoved', parkNumber)
    }
  }
}
</script>

<style>
  #park-list-container {
    display: flex;
    flex-wrap: wrap;
  }
  .list-item {
    box-sizing: border-box;
    display: flex;
    align-items: center;
    width: 300px;
  }
  #park-list-name {
    margin-left: 10px;
  }
  #remove-button {
    margin-left: 10px;
  }
</style>
