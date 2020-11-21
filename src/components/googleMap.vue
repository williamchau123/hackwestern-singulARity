<template>
  <GmapMap
  :center="{lat:10, lng:10}"
  :zoom="1"
  map-type-id="terrain"
  style="width: 100%; height: 100vh"
  @drag="updateCoordinates"
  @click="mark"
>
  <GmapMarker
    :key="index"
    v-for="(m, index) in markers"
    :position="m.position"
    :clickable="true"
    :draggable="true"
    @drag="moveInfoWindow(m)"
    @click="toggleInfoWindow(m, index)"
  />
  <GmapInfoWindow :options="infoOptions" :position="infoWindowPos" :opened="infoWinOpen" @closeclick="infoWinOpen=false"></GmapInfoWindow>
  <GmapMarker ref="myMarker"
    :position="google && new google.maps.LatLng(1.38, 103.8)"
    :draggable="true" 
    :clickable="true"
    icon="https://developers.google.com/maps/documentation/javascript/examples/full/images/beachflag.png"/>
</GmapMap>
</template>

<script>
  import {gmapApi} from 'vue2-google-maps'
  export default {
    data() {
      
        return {
          markers: [
          ],
          infoWindowPos: null,
          infoWinOpen: false,
          currentMidx: null,

          infoOptions: {
            content: '',
            //optional: offset infowindow so it visually sits nicely on top of our marker
            pixelOffset: {
              width: 0,
              height: -35
            }
          },

          coordinates: null,
        }
    },
    methods: {
        mark(event) {
            console.log(event.latLng.lat())
            console.log(event.latLng.lng())
            this.markers.push({position:{lat:event.latLng.lat(), lng:event.latLng.lng()}, infoText:`${event.latLng.lat()}, ${event.latLng.lng()}`})
        },
        toggleInfoWindow(marker, idx) {
            this.infoWindowPos = marker.position;
            this.infoOptions.content = marker.infoText;

            //check if its the same marker that was selected if yes toggle
            if (this.currentMidx == idx) {
              this.infoWinOpen = !this.infoWinOpen;
            }
            //if different marker set infowindow to open and reset current marker index
            else {
              this.infoWinOpen = true;
              this.currentMidx = idx;

            }
          },
        moveInfoWindow(marker) {
            console.log(marker)
            console.log(event)
            marker.position = {lat: event.latLng.lat(), lng:event.latLng.lng()}
        },
        updateCoordinates(location) {
            this.coordinates = {
                lat: location.latLng.lat(),
                lng: location.latLng.lng(),
            };
        },
    },
    computed: {
      google: gmapApi
    }
  }
</script>
