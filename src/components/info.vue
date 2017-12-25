<template>
  <div class="info-container" :addedParks="addedParks" :parks="parks">
    <div class="park-info-box" v-if="currentParkIndex == 'preload'">
      <p class="park-name">Click on a map marker to select a park</p>
    </div>
    <div class="park-info-box" v-else>
      <img :src="parks[currentParkIndex].image" :alt="parks[currentParkIndex].name">
      <div class="park-column">
        <div class="park-title-row">
          <p class="park-name"> {{ parks[currentParkIndex].name }} </p>
          <button class="colored-button" @click="addPark(parks[currentParkIndex].number)" v-if="!parks[currentParkIndex].added">Add to route</button>
          <button class="colored-button" @click="removePark(parks[currentParkIndex].number)" v-else>Remove from route</button>
        </div>
        <p class="description"> {{ parks[currentParkIndex].description }} </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['parks', 'addedParks'],
  methods: {
    updateIndex: function (parksIndex) {
      // updates the index with the clicked park
      this.currentParkIndex = parksIndex
    },
    addPark: function (parkNumber) {
      // emits the parkAdded event and passes the number property of the park
      this.$evt.$emit('parkAdded', parkNumber)
    },
    removePark: function (parkNumber) {
      // emits the parkRemoved event and passes the number property of the park
      this.$evt.$emit('parkRemoved', parkNumber)
    }
  },
  mounted () {
    console.log('info -> mounted')
  // listens for a marker to be clicked, updates index
    this.$evt.$on('markerClicked', this.updateIndex)
  },
  beforeDestroy () {
    console.log('info -> beforeDestroy')
    this.$evt.$off('markerClicked', this.updateIndex)
  },
  data () {
    return {
      currentParkIndex: 'preload'
    }
  }
}
</script>
<style>
.info-container {
  margin-top: 25px;
}

.park-info-box {
  display: flex;
  flex-direction: row;
  margin: 45px 25px;
}

.park-column {
  margin-left: 35px;
  display: flex;
  flex-direction: column;
  width: 60%;
}

.park-title-row {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: flex-start;
  width: 100%;
  margin-bottom: 20px;
}

.park-name {
  font-size: 1.5em;
  color: #143a44;
  font-weight: bold;
  text-align: left;
}

.description {
  text-align: left;
}

@media(max-width: 769px) {
  .park-info-box {
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .park-column {
    margin-top: 20px;
    margin-left: 0px;
    width: auto;
  }

  .park-name {
    font-size: 1.4em;
  }

  .park-info-box {
    margin-top: 25px;
    margin-bottom: 25px;
  }
}

</style>
