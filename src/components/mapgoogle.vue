<!-- some of this code comes from https://developers.google.com/maps/documentation/javascript/examples/directions-waypoints,
google code was adapted to suit vue, webpack, and this project. -->
<template>
  <div id="map-wrapper" class="column is-half" :addedParks="addedParks" :parks="parks">
    <div id="map-inner-wrapper">
      <div id="map"></div>
    </div>
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
      start: 'Chapel Hill, NC',
      end: 'Chapel Hill, NC'
    }
  },
  props:
    ['parks', 'addedParks'],
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
        center: {lat: 35.40, lng: -79.78}
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
    // distance and duration code written by me though
      this.polyline = new google.maps.Polyline({
        path: [],
        strokeColor: '#FF0000',
        strokeWeight: 3
      })
      var bounds = new google.maps.LatLngBounds()
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
    updateWaypoints: function (parkIndex) {
      var waypoints = []
      console.log(this.addedParks)
      for (var q in this.addedParks) {
        console.log(this.addedParks[q])
        var waypoint = {
          location: this.addedParks[q].name,
          stopover: true
        }
         waypoints.push(waypoint)
      }
      console.log(waypoints)
      this.waypts = waypoints
      console.log(this.waypts)
    },
    setStart: function (start) {
      this.start = start
    },
    setEnd: function (end) {
      this.end = end
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
    this.$evt.$on('parkAddComplete', this.updateWaypoints)
    // listens for a park to be removed, then launches removeClickedPark
    this.$evt.$on('parkRemoveComplete', this.updateWaypoints)
    // listens for the response from google, then draws the polyline and gets directions
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
     #map-wrapper {
       min-height: 100%;
       margin-left: 25px;
     }
     #map {
       height: 275px;
     }
     #map-inner-wrapper {
       height: 100%;
       flex: 1;
     }
</style>
