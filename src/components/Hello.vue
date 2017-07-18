<template>
  <div class="hello">
    <div id="map"></div>
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
    initPosition () {
      for (let i = -180; i < 180; i++) {
        this.latitude.push({lat: 30.000, lng: parseFloat(i)});
      }
      this.test();
    },
    saveAltitude (elevations, status) {

    },
    getAltitude () {

    },
    displayLocationElevation (location, elevator, infowindow) {
      elevator.getElevationForLocations({
          'locations': [location]
        }, function(results, status) {
          infowindow.setPosition(location);
          if (status === 'OK') {
            // Retrieve the first result
            if (results[0]) {
              // Open the infowindow indicating the elevation at the clicked position.
              infowindow.setContent('The elevation at this point <br>is ' +
                  results[0].elevation + ' meters.');
            } else {
              infowindow.setContent('No results found');
            }
          } else {
            infowindow.setContent('Elevation service failed due to: ' + status);
          }
        });
    },
    test () {
      let that = this;
      var uluru = {lat: -25.363, lng: 131.044}
      var map = new window.google.maps.Map(document.getElementById('map'), {
        zoom: 4,
        center: this.latitude[1]
      });
      let elevator = new google.maps.ElevationService;
      var infowindow = new google.maps.InfoWindow({map: map});

      // Add a listener for the click event. Display the elevation for the LatLng of
      // the click inside the infowindow.
      map.addListener('click', function(event) {
        that.displayLocationElevation(event.latLng, elevator, infowindow);
      });
    }
  },
  mounted () {
    this.initPosition()
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

#map {
  height: 400px;
  width: 100%;
}
</style>
