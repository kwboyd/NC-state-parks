<template>
  <div :parks="parks" id="container">
      <h1> {{ parks[currentParkIndex].name }} </h1>
      <h2> {{ parks[currentParkIndex].description }} </h2>
      <button class="btn" @click="addPark" v-show="!parks[currentParkIndex].added">Add</button>
      <button class="btn" @click="removePark" v-show="parks[currentParkIndex].added">Remove</button>
  </div>
</template>
<script>
export default {
  props: {
    parks: {
      //defaults for before parks json is loaded
      default: [{
        "name": '',
        "description": '',
        "coords": ''
      }]
    }
},
  // computed: {
  //   parkStatus: function() {
  //       return parks[currentParkIndex].added
  //   }
  // },
  methods: {
    updateIndex: function(parksIndex) {
      //updates the index with the clicked park
      this.currentParkIndex = parksIndex
      console.log(this.currentParkIndex)
    },
    addPark: function () {
      //emits the parkAdded event and passes the index of the park
      this.$evt.$emit('parkAdded', this.currentParkIndex)
      this.parks[this.currentParkIndex].added = true
    },
    removePark: function() {
      //emits the parkRemoved event and passes the index of the park
      this.$evt.$emit('parkRemoved', this.currentParkIndex)
      this.parks[this.currentParkIndex].added = false
    }
  },
  mounted() {
  console.log('info -> mounted')
  //listens for a marker to be clicked, updates index
  this.$evt.$on('markerClicked', this.updateIndex)
},
data () {
  return {
    currentParkIndex: 0,
  }
}
}
</script>
<style scoped>
 #container {
  position: absolute;
   top: 500px;
 }
 .button {
   padding: 10px;
   background-color: blue;
   color: white;
 }
</style>
