<template>
  <div class="hello">
    <div id="gmap"></div>
   <!--  <button @click="test">click</button> -->
    <h2 v-if="error">{failStatus}</h2>
    <ul v-else v-for="item in showLatitude">
      <li>
        {item.elevation}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      latitude: [],
      error: false,
      failStatus: '',
      showLatitude: [],
      elevator: '',
      map: ''
    }
  },
  methods: {
    // initPosition () {
    //   for (let i = -180; i <= 180; i++) {
    //     this.latitude.push(new window.google.maps.LatLng(30.000, 22))
    //   }
    // },
    // saveAltitude (elevations, status) {
    //   console.log(status)
    //   if (status !== 'OK') {
    //     this.error = true
    //     this.failStatus = 'Cannot show elevation: request failed because ' + status
    //     return
    //   }
    //   this.showLatitude = elevations.slice()
    // },
    // getAltitude () {
    //   this.elevator = new window.google.maps.ElevationService()
    //   let infowindow = new window.google.maps.InfoWindow({map: this.map})
    //   this.map.addListener('click', function (event) {
    //     this.displayLocationElevation(event.latLng, this.elevator, infowindow)
    //   })
    // },
    // displayLocationElevation (location, elevator, infowindow) {
    //   this.elevator.getElevationForLocations({
    //     'locations': [location]
    //   }, function (results, status) {
    //     infowindow.setPosition(location)
    //     if (status === 'OK') {
    //       // Retrieve the first result
    //       if (results[0]) {
    //         // Open the infowindow indicating the elevation at the clicked position.
    //         infowindow.setContent('The elevation at this point <br>is ' +
    //             results[0].elevation + ' meters.')
    //       } else {
    //         infowindow.setContent('No results found')
    //       }
    //     } else {
    //       infowindow.setContent('Elevation service failed due to: ' + status)
    //     }
    //   })
    // },
    createMap () {
      console.log(window.google.maps)
      // console.log(JSON.stringify(window.google.maps.Map));
      new window.google.maps.Map(document.getElementById('gmap'), {
        center: {
          zoom: 8,
          mapTypeId: 'terrain'
        }
      })
    }
  },
  created () {
    // this.initPosition()
    this.createMap()
    // this.getAltitude()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
