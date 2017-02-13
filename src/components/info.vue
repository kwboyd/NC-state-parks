<template>
  <div :parks="parks" id="container">
      <h1> {{ parks[currentParkIndex].name }} </h1>
      <h2> {{ parks[currentParkIndex].description }} </h2>
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
  methods: {
    updateIndex: function(parksIndex) {
      //updates the index with the clicked park
      this.currentParkIndex = parksIndex
      console.log(this.currentParkIndex)
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
</style>
