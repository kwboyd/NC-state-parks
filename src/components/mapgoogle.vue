<!-- much of this code comes from https://developers.google.com/maps/documentation/javascript/examples/directions-waypoints,
google code was adapted to suit vue, webpack, and this project -->
<template>
<div :parks="parks">
  <div id="right-panel">
  <div>
  <b>Start:</b>
  <select id="start">
    <option value="Halifax, NS">Halifax, NS</option>
    <option value="Boston, MA">Boston, MA</option>
    <option value="New York, NY">New York, NY</option>
    <option value="Miami, FL">Miami, FL</option>
  </select>
  <br>
  <b>Waypoints:</b> <br>
  <i>(Ctrl+Click or Cmd+Click for multiple selection)</i> <br>
  <select multiple id="waypoints">
      <option v-for="park in parks" :value="park.name">{{ park.name }}</option>
  </select>
  <br>
  <b>End:</b>
  <select id="end">
    <option value="Vancouver, BC">Vancouver, BC</option>
    <option value="Seattle, WA">Seattle, WA</option>
    <option value="San Francisco, CA">San Francisco, CA</option>
    <option value="Los Angeles, CA">Los Angeles, CA</option>
  </select>
  <br>
    <input @click="createMap()" type="submit" id="submit">
  </div>
  <div id="directions-panel"></div>
  </div>
</div>
</template>
<script>
import wpoption from './wpoption'
export default {
  data () {
    return {
      directionsDisplay: '',
      directionsService: '',
      dataLoadComplete: false,
      locations: [],
      markers: []
      }
  },
  props:
    ['parks'],
  components: {
    wpoption
  },
  methods: {
    pushMarkers: function () {
      //pushes coordinates from parks to locations index
      for (var i in this.parks) {
        this.locations.push(this.parks[i].coords)
      }
      console.log(this.locations)
      this.initMap()
    },
    initMap: function () {
    // creates the inital map display
      console.log('inited')
      var map = new google.maps.Map(document.getElementById('map'), {
        zoom: 6,
        center: {lat: 41.85, lng: -87.65}
      })

      this.directionsService = new google.maps.DirectionsService
      this.directionsDisplay = new google.maps.DirectionsRenderer
      for (var location in this.locations) {
        //pushes coords from locations to markers and creates markers via google
        this.markers.push (new google.maps.Marker({
          position: this.locations[location],
          map: map,
          animation: google.maps.Animation.DROP
        }))
      }
      this.directionsDisplay.setMap(map)
    },
      createMap: function () {
        //draws the route and displays directions
        console.log('clicked')
        var waypts = []
        var checkboxArray = document.getElementById('waypoints')
          for (var i = 0; i < checkboxArray.length; i++) {
            if (checkboxArray.options[i].selected) {
              waypts.push({
                location: checkboxArray[i].value,
                stopover: true
              })
            }
          }
        var currentService = this.directionsService
        var currentDisplay = this.directionsDisplay
        currentService.route({
          //draws the route on the map
          origin: document.getElementById('start').value,
          destination: document.getElementById('end').value,
          waypoints: waypts,
          optimizeWaypoints: true,
          travelMode: 'DRIVING'
          }, function (response, status) {
        if (status === 'OK') {
              currentDisplay.setDirections(response)
              var route = response.routes[0]
              var summaryPanel = document.getElementById('directions-panel')
              summaryPanel.innerHTML = ''
              // For each route, display summary information.
              for (var i = 0; i < route.legs.length; i++) {
                var routeSegment = i + 1
                summaryPanel.innerHTML += '<b>Route Segment: ' + routeSegment +
                    '</b><br>'
                summaryPanel.innerHTML += route.legs[i].start_address + ' to '
                summaryPanel.innerHTML += route.legs[i].end_address + '<br>'
                summaryPanel.innerHTML += route.legs[i].distance.text + '<br><br>'
              }
            } else {
              window.alert('Directions request failed due to ' + status)
            }
        })
      }
  },
  mounted () {
    console.log('googlemap -> mounted')
    //listens for dataLoaded event
    this.$evt.$on('dataLoaded', (function (){
      this.$nextTick(() =>
      //after the next 'tick' to the dom, emits dataLoadComplete event
      this.$emit('dataLoadComplete')
    )
    }))
    //listens for dataLoadComplete event, then launches initMap
    this.$evt.$on('dataLoadComplete', this.pushMarkers)
},
  beforeDestroy () {
    console.log ('mapgoogle -> beforeDestroy')
    this.$evt.$off('dataLoadComplete', this.pushMarkers)
    this.$evt.$off('dataLoaded')
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
</style>
