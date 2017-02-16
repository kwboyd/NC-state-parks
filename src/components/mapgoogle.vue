<!-- some of this code comes from https://developers.google.com/maps/documentation/javascript/examples/directions-waypoints,
google code was adapted to suit vue, webpack, and this project. -->
<template>
  <div id="map-container" class="column is-half" :addedParks="addedParks" :parks="parks">
    <div id="map-wrapper">
      <div id="map-inner-wrapper">
        <div id="map"></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      directionsDisplay: '',
      directionsService: '',
      locations: [],
      markers: [],
      map: '',
      waypts: [],
      polyline: [],
      startPlace: 'Chapel Hill, NC',
      endPlace: 'Chapel Hill, NC'
    }
  },
  props:
    ['parks', 'addedParks'],
  methods: {
    pushMarkers: function () {
      // pushes coordinates from parks to locations index
      for (var parksIndex in this.parks) {
        this.locations.push(this.parks[parksIndex].coords)
      }
      this.polyline = new window.google.maps.Polyline({
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
          window.google.maps.event.addListener(this.markers[parksIndex], 'click', () => {
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
      this.map = new window.google.maps.Map(document.getElementById('map'), {
        zoom: 6,
        center: {lat: 35.40, lng: -79.78}
      })
      // loads the DirectionsService and DirectionsRenderer from google
      this.directionsService = new window.google.maps.DirectionsService()
      this.directionsDisplay = new window.google.maps.DirectionsRenderer()
      for (var i in this.locations) {
        // pushes coords from locations to markers and creates markers via google
        var marker = (new window.google.maps.Marker({
          position: this.locations[i],
          map: this.map,
          animation: window.google.maps.Animation.DROP
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
    // distance and duration code written by me though
      this.polyline = new window.google.maps.Polyline({
        path: [],
        strokeColor: '#FF0000',
        strokeWeight: 3
      })
      var bounds = new window.google.maps.LatLngBounds()
      var legs = response.routes[0].legs
      var currentDistance = 0
      var currentDuration = 0
      for (var q = 0; q < legs.length; q++) {
        currentDistance = currentDistance + legs[q].distance.value
        currentDuration = currentDuration + legs[q].duration.value
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
      this.$evt.$emit('legsUpdated', currentDistance, currentDuration)
    },
    createMap: function () {
        // draws the route and displays directions
        // sets 'this' to self so that 'this' can be used in the inline callback function
      var self = this
        // renames directionsService and directionsDisplay so it can be passed as arguments easier
      var currentService = self.directionsService
      var currentDisplay = self.directionsDisplay
      currentService.route({
          // sets the route via google
        origin: this.startPlace,
        destination: this.endPlace,
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
    updateWaypoints: function () {
      // updates the waypoints array for google
      var waypoints = []
      for (var i in this.addedParks) {
        // loops through addedParks and adds each as a waypoint
        var waypoint = {
          location: this.addedParks[i].name,
          stopover: true
        }
        waypoints.push(waypoint)
      }
      this.waypts = waypoints
    },
    setStart: function (startPlace) {
      // updates the starting location
      this.startPlace = startPlace
    },
    setEnd: function (endPlace) {
      // updates the ending location
      this.endPlace = endPlace
    }
  },
  mounted () {
    console.log('googlemap -> mounted')
    this.$evt.$on('dataLoaded', function () {
      this.$nextTick(() =>
      // after the next 'tick'/change to the dom, emits dataLoadComplete event
      // this avoids the dataLoaded event being emited too early before axios is complete
      // setting defaults for parks and then calling pushMarkers after dataLoaded is emitted does not work
      this.$evt.$emit('dataLoadComplete')
    )
    })
    this.$evt.$on('dataLoadComplete', this.pushMarkers)
    this.$evt.$on('parkAddComplete', this.updateWaypoints)
    this.$evt.$on('parkRemoveComplete', this.updateWaypoints)
    this.$evt.$on('responseOk', this.drawLine)
    this.$evt.$on('startUpdated', this.setStart)
    this.$evt.$on('endUpdated', this.setEnd)
    this.$evt.$on('updateClicked', this.createMap)
  },
  beforeDestroy () {
    console.log('mapgoogle -> beforeDestroy')
    this.$evt.$off('dataLoadComplete', this.pushMarkers)
    this.$evt.$off('dataLoaded')
    this.$evt.$off('parkAddComplete', this.updateWaypoints)
    this.$evt.$off('parkRemoveComplete', this.updateWaypoints)
    this.$evt.$off('responseOk', this.drawLine)
    this.$evt.$off('startUpdated', this.setStart)
    this.$evt.$off('endUpdated', this.setEnd)
    this.$evt.$off('updateClicked', this.createMap)
  }
}
</script>
<style>
    #map-container {
      border-radius: 8px;
      background-color: #fff;
    }
     #map-wrapper {
       min-height: 100%;
       margin-left: 25px;
       padding: 0;
       margin-right: 12px;
       border-radius: 8px;
     }
     #map {
       height: 275px;
     }
     #map-inner-wrapper {
       height: 100%;
       flex: 1;
     }
</style>
