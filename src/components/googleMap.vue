<template>
  <div>
    <GmapMap
      :center="{ lat: this.user.position.lat, lng: this.user.position.lng }"
      :zoom="12"
      map-type-id="terrain"
      style="width: 100%; height: 100vh"
    >
      <!-- @drag="updateCoordinates" -->
      <!-- @click="mark" -->
      <GmapMarker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        :clickable="true"
        :icon="m.iconPic"
        @click="toggleInfoWindow(m, index)"
      />
      <GmapInfoWindow
        :options="infoOptions"
        :position="infoWindowPos"
        :opened="infoWinOpen"
        @closeclick="infoWinOpen = false"
      ></GmapInfoWindow>
      <GmapMarker
        ref="user"
        :position="this.user.position"
        :clickable="true"
        icon="https://developers.google.com/maps/documentation/javascript/examples/full/images/beachflag.png"
        @click="toggleInfoWindow(user, -1)"
      />
    </GmapMap>
  </div>
</template>

<script>
import { gmapApi } from "vue2-google-maps";
import axios from "axios";
export default {
  async mounted() {
    navigator.geolocation.watchPosition(
      (position) => {
        this.user.position.lat = position.coords.latitude;
        this.user.position.lng = position.coords.longitude;
      },
      (error) => {
        console.log(error.message);
      }
    );
    let data = { message: "Hi" };
    let response = await axios.post(
      "https://us-central1-hackwestern7-296318.cloudfunctions.net/get-ar-data",
      data
    );
    response = response.data;
    console.log(response);
    for (const k in response) {
      const value = response[k];
      let iconPic = "";
      if (value.type === "available") {
        iconPic = "https://i.imgur.com/jxQLWJ3.png?1";
      } else if (value.type === "unavailable") {
        iconPic = "https://i.imgur.com/IgAzA6P.png";
      } else {
        iconPic = "https://i.imgur.com/TKr8kjn.png?1";
      }

      // console.log(value.lat,value.lng, k, value)
      this.markers.push({
        position: { lat: parseFloat(value.lat), lng: parseFloat(value.lng) },
        infoText: value.description,
        iconPic,
        title: value.name,
        url: value.url,
        type: value.type
      });
      k;
    }
  },
  data() {
    return {
      user: { position:{lat: 0, lng: 0}, infoText:"This is you!!", title:"YOU"},
      markers: [],
      infoWindowPos: null,
      infoWinOpen: false,
      currentMidx: null,

      infoOptions: {
        content: "",
        //optional: offset infowindow so it visually sits nicely on top of our marker
        pixelOffset: {
          width: 0,
          height: -35,
        },
      },

      coordinates: null,
    };
  },
  methods: {
    mark(event) {
      console.log(event.latLng.lat());
      console.log(event.latLng.lng());
      this.markers.push({
        position: { lat: event.latLng.lat(), lng: event.latLng.lng() },
        infoText: `${event.latLng.lat()}, ${event.latLng.lng()}`,
      });
    },
    getInfoWindowContent(marker) {
      let text = ""
      if (marker.type === "available"){
        text =     
        `<div>
          <h2>
            ${marker.title}
          </h2>
          <p>
            ${marker.infoText}
          </p>
          <a href=${marker.url}>
            Click to reveal
          </a>
        </div>`
      }
      else if (marker.type === "unavailable"){
        text =     
        `<div>
          <h2>
            ${marker.title}
          </h2>
          <p>
            ${marker.infoText}
          </p>
          <p>
            Come closer to reveal the AR tag
          </p>
        </div>`
      }
      else {
          text =     
        `<div>
          <h2>
            ${marker.title}
          </h2>
          <p>
            ${marker.infoText}
          </p>
        </div>`
      }
      
      return text
    },
    toggleInfoWindow(marker, idx) {
      this.infoWindowPos = marker.position;
      this.infoOptions.content = this.getInfoWindowContent(marker);

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
    updateCoordinates(location) {
      this.coordinates = {
        lat: location.latLng.lat(),
        lng: location.latLng.lng(),
      };
    },
  },
  computed: {
    google: gmapApi,
  },
};
</script>
