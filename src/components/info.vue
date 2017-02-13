<template>
  <div :currentParkIndex="currentParkIndex" :parks="parks" id="container">
      <h1> {{ parks[currentIndex] }} </h1>
      <!-- <h2> {{ parks[currentParkIndex].description }} </h2> -->
  </div>
</template>
<script>
export default {
  props: {
    parks: {
      default: [{
        "name": '',
        "description": '',
        "coords": ''
      }]
    },
  currentParkIndex: {
    default: "0"
  }
},
  methods: {
    updateParks: function() {
      this.parkData = this.parks
      console.log(this.parkData)
    }
  },
  mounted() {
  this.$evt.$on('dataLoadComplete', this.updateParks)
},
data () {
  return {
    currentIndex: 0,
    parkData: [{
      "name": '',
      "description": '',
      "coords": ''
    }]
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
