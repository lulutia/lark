<template>
  <div class="hello">
    <div id="three"></div>
    <h2 v-if="error">{{failStatus}}</h2>
    <ul v-else v-for="item in showLatitude">
      <li>
        {{item.elevation}}
      </li>
    </ul>
  </div>
</template>

<script>
import * as THREE from 'three';
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
      for (let i = -1; i < 10; i++) {
        this.latitude.push({lat: 30.000, lng: parseFloat(i)});
      }
      this.getAltitude();
    },
    saveAltitude (results, status) {
      if (status === 'OK') {
        if (results) {
          this.showLatitude = results.slice();
          this.renderScene();
        } else {
          console.log('no results');
        }
      } else {
        this.failStatus = status;
        this.error = true;
      }
    },
    renderScene () {
      let scene, camera, renderer;
      let geometry, material, mesh;
      console.log(THREE);
      scene = new THREE.Scene();

      camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 );
      camera.position.z = 500;

      geometry = new THREE.BoxGeometry( 200, 200, 200 );
      material = new THREE.MeshBasicMaterial( { color: 0xff0000, wireframe: true } );

      mesh = new THREE.Mesh( geometry, material );
      scene.add( mesh );

      renderer = new THREE.WebGLRenderer();
      renderer.setSize( window.innerWidth, window.innerHeight );

      document.getElementById('three').appendChild(renderer.domElement);
      this.animate(mesh, renderer, scene, camera);
    },
    animate (mesh, renderer, scene, camera) {
      //console.log(mesh.rotation);
      mesh.rotation.x += 0.01;
      mesh.rotation.y += 0.02;
      renderer.render( scene, camera);
      //requestAnimationFrame(this.animate(mesh, renderer, scene, camera));
    },
    getAltitude () {
      let elevator = new google.maps.ElevationService;
      elevator.getElevationForLocations({
        'locations': this.latitude
      }, this.saveAltitude)
    },
    displayLocationElevation (location, elevator, infowindow) {
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

</style>
