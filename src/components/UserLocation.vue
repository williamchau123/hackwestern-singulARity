<template>
  <div id="app">
    <p>Lat = {{ lat }} Lon ={{ lon }}</p>
    <p>{{ error }}</p>
    <button @click="myFunction()">Click Me</button>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      error: "",
      lat: "",
      lon: "",
    };
  },
  methods: {
    myFunction: function () {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.showPosition);
      } else {
        this.error = "Geolocation is not supported.";
      }
    },
    showPosition: function (position) {
      this.lat = position.coords.latitude;
      this.lon = position.coords.longitude;
    },
  },
  async mounted() {
    navigator.geolocation.getCurrentPosition(this.showPosition);
    // let header = {
    //       // mode: 'no-cors',
    //       headers: {
    //         'mode': 'no-cors',
    //         'Access-Control-Allow-Headers': '*',
    //         'Access-Control-Allow-Origin': '*',
    //         'Content-Type': 'application/json',
    //         'Access-Control-Allow-Methods': 'GET,POST,',
    //       }}
    let data = {message:"Hi"}
    await axios
      .post(
        'https://us-central1-hackwestern7-296318.cloudfunctions.net/receiveUserLocation-1', 
        data,
        // header
        )
      .then(response => (console.log(response)))
  },
};
</script>

<style>
</style>