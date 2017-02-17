<template>
  <div class="level">
    <div class="level-item question">
      <p>Starting location: &nbsp</p>
      <input class="place-input" v-model="startPlace" placeholder="City, State OR Zip">
    </div>
    <div class="level-item question">
      <p id="ending-location">Ending location: &nbsp</p>
      <input class="place-input" v-model="endPlace" placeholder="City, State OR Zip">
    </div>
    <div class="level-item">
      <button class="button" @click="updateLocations">Update locations</button>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      startPlace: 'Chapel Hill, NC',
      endPlace: '27514'
    }
  },
  methods: {
    updateLocations () {
      // emits an event that the locations have been updated, passes endPlace and startPlace
      this.$evt.$emit('locationsUpdated', this.endPlace, this.startPlace)
    }
  }
}
</script>

<style>
.question {
  color: white;
  font-size: 1.5em;
  font-weight: bold;
}

.place-input {
  align-self: stretch;
  padding-left: 5px;
}
@media(max-width: 769px) {
  #ending-location {
  margin-left: -10px;
  }
}
</style>
