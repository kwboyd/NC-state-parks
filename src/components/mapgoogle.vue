<!-- some of this code comes from https://developers.google.com/maps/documentation/javascript/examples/directions-waypoints,
google code was adapted to suit vue, webpack, and this project. -->
<template>
<div :parks="parks">
  <div id="right-panel">
  <div>
  <b>Start:</b>
  <input v-model="start" placeholder="City, State OR Zip">
  <br>
  <b>End:</b>
  <input v-model="end" placeholder="City, State OR Zip">
  <br>
  </div>
  <div id="directions-panel"></div>
  </div>
  <button class="btn" @click="createMap()">Update Map </button>
</div>
</template>
<script>
export default {
  data () {
    return {
      directionsDisplay: '',
      directionsService: '',
      dataLoadComplete: false,
      locations: [],
      markers: [],
      map: '',
      currentPolyline: '',
      addedParkIndex: '',
      waypts: [],
      polyline: [],
      currentDisplay: '',
      start: '',
      end: ''
    }
  },
  props:
    ['parks'],
  methods: {
    pushMarkers: function () {
      // pushes coordinates from parks to locations index
      for (var i in this.parks) {
        this.locations.push(this.parks[i].coords)
      }
      this.polyline = new google.maps.Polyline({
        path: [],
        strokeColor: '#FF0000',
        strokeWeight: 3
      })
      this.initMap()
    },
    addListeners () {
      // uses google's addListener method to add events to the markers
      for (var parksIndex in this.markers) {
        ((parksIndex) => {
          google.maps.event.addListener(this.markers[parksIndex], 'click', () => {
          // emits markerClicked, passes parksIndex
            this.$evt.$emit('markerClicked', parksIndex)
          })
        })(parksIndex)
      }
    },
    initMap: function () {
    // creates the inital map display
      console.log('inited')
      // gets the map div and initializes a google map
      this.map = new google.maps.Map(document.getElementById('map'), {
        zoom: 7,
        center: {lat: 35.91, lng: -79.05}
      })
      // loads the DirectionsService and DirectionsRenderer from google
      this.directionsService = new google.maps.DirectionsService
      this.directionsDisplay = new google.maps.DirectionsRenderer
      for (var location in this.locations) {
        // pushes coords from locations to markers and creates markers via google
        var marker = (new google.maps.Marker({
          position: this.locations[location],
          map: this.map,
          animation: google.maps.Animation.DROP
        }))
        this.markers.push(marker)
      }
      // draws the map
      this.directionsDisplay.setMap(this.map)
      this.addListeners()
    },
    drawLine: function (response, status, currentDisplay) {
    // resets and then draws the polyline
    // clears out the old polyline
      this.polyline.setMap(null)
    // this code from http://stackoverflow.com/questions/16180104/get-a-polyline-from-google-maps-directions-v3
      this.polyline = new google.maps.Polyline({
        path: [],
        strokeColor: '#FF0000',
        strokeWeight: 3
      })
      var bounds = new google.maps.LatLngBounds()

      var legs = response.routes[0].legs
      for (var q = 0; q < legs.length; q++) {
        var steps = legs[q].steps
        for (var j = 0; j < steps.length; j++) {
          var nextSegment = steps[j].path
          for (var k = 0; k < nextSegment.length; k++) {
            this.polyline.getPath().push(nextSegment[k])
            bounds.extend(nextSegment[k])
          }
        }
      }
    // end code from stackoverflow
    // draws the polyline for the route
      this.polyline.setMap(currentDisplay.map)
    },
    createMap: function () {
        // draws the route and displays directions
      console.log('submitted')
        // sets 'this' to self so that 'this' can be used in the inline callback function
      var self = this
        // renames directionsService and directionsDisplay so it can be passed as arguments easier
      var currentService = self.directionsService
      var currentDisplay = self.directionsDisplay
      currentService.route({
          // sets the route via google
        origin: this.start,
        destination: this.end,
        waypoints: self.waypts,
        optimizeWaypoints: true,
        travelMode: 'DRIVING'
      }, function (response, status) {
        if (status === 'OK') {
          // emits a response because the scope is too narrow inside this function to call a method
          self.$evt.$emit('responseOk', response, status, currentDisplay)
                  // currentDisplay.setDirections(response)
              // displays the route
            // var route = response.routes[0]
              // var summaryPanel = document.getElementById('directions-panel')
              // summaryPanel.innerHTML = ''
              // For each route, display summary information.
              // for (var i = 0; i < route.legs.length; i++) {
              // var routeSegment = i + 1
              // summaryPanel.innerHTML += '<b>Route Segment: ' + routeSegment +
              // '</b><br>'
              // summaryPanel.innerHTML += route.legs[i].start_address + ' to '
              // summaryPanel.innerHTML += route.legs[i].end_address + '<br>'
              // summaryPanel.innerHTML += route.legs[i].distance.text + '<br><br>'
              // }
        } else {
          window.alert('Directions request failed due to ' + status)
        }
      })
    },
    addClickedPark: function (currentParkIndex) {
    // adds the park to the waypoints array
      this.waypts.push({
        location: this.parks[currentParkIndex].name,
        stopover: true})
      this.$evt.$emit('wayptAdded', this.waypts)
    },
    removeClickedPark: function (currentParkIndex) {
    // removes the parks from the waypoints array at the current index
      this.waypts.splice(currentParkIndex, 1)
      this.$evt.$emit('wayptRemoved', this.waypts)
    }
  },
  mounted () {
    console.log('googlemap -> mounted')
    // listens for dataLoaded event
    this.$evt.$on('dataLoaded', function () {
      this.$nextTick(() =>
      // after the next 'tick'/change to the dom, emits dataLoadComplete event
      // this avoids the dataLoaded event being emited too early before axios is complete
      // setting defaults for parks and then calling pushMarkers after dataLoaded is emitted does not work
      this.$evt.$emit('dataLoadComplete')
    )
    })
    // listens for dataLoadComplete event, then launches initMap
    this.$evt.$on('dataLoadComplete', this.pushMarkers)
    // listens for a park to be added, then launches addClickedPark
    this.$evt.$on('parkAdded', this.addClickedPark)
    // listens for a park to be removed, then launches removeClickedPark
    this.$evt.$on('parkRemoved', this.removeClickedPark)
    // listens for the response from google, then draws the polyline and gets directions
    this.$evt.$on('responseOk', this.drawLine)
  },
  beforeDestroy () {
    console.log('mapgoogle -> beforeDestroy')
    this.$evt.$off('dataLoadComplete', this.pushMarkers)
    this.$evt.$off('dataLoaded')
    this.$evt.$off('parkAdded', this.addClickedPark)
    this.$evt.$off('parkRemoved', this.removeClickedPark)
    this.$evt.$off('responseOk', this.drawLine)
  }
}
</script>
<style>
#right-panel {
       font-family: 'Roboto','sans-serif';
       line-height: 30px;
       padding-left: 10px;
     }

     #right-panel select, #right-panel input {
       font-size: 15px;
     }

     #right-panel select {
       width: 100%;
     }

     #right-panel i {
       font-size: 12px;
     }
     html, body {
       height: 100%;
       margin: 0;
       padding: 0;
     }
     #map {
       height: 100%;
       float: left;
       width: 70%;
       height: 100%;
     }
     #right-panel {
       margin: 20px;
       border-width: 2px;
       width: 20%;
       height: 400px;
       float: left;
       text-align: left;
       padding-top: 0;
     }
     #directions-panel {
       margin-top: 10px;
       background-color: #FFEE77;
       padding: 10px;
     }
     .btn {
       padding: 10px;
       background-color: teal;
       color: white;
       float: left;
       margin-left: 30px;
     }
</style>
