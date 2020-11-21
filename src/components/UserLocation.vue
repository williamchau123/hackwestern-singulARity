<template>
  <div id="app">
    <p>Lat = {{ lat }} Lon ={{ lon }}</p>
    <p>{{ error }}</p>
    <button @click="myFunction()">Click Me</button>
  </div>
</template>

<script>
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
};
</script>

<style>
</style>