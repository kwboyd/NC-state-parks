<template>
  <div class="column" :addedParks="addedParks">
    <!-- creates list of added parks -->
    <ul class="park-list-container">
      <!-- if the addedParks array isn't empty, display that park in the list -->
      <div class="list-item" v-for="park in addedParks" v-show="addedParks.length > 0">
        <!-- passes the number property of the clicked park, which equals the park's index in the parks array -->
        <button class="remove-button colored-button" @click="removeParkFromList(park.number)">Remove</button>
        <p class="park-list-name">{{ park.name }}</p>
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
.park-list-container {
  display: flex;
  flex-wrap: wrap;
}

.list-item {
  box-sizing: border-box;
  display: flex;
  align-items: center;
  width: 310px;
  margin-top: 3px;
}

.park-list-name {
  font-weight: 500;
  margin-left: 10px;
}

.remove-button {
  margin-left: 10px;
}
</style>
