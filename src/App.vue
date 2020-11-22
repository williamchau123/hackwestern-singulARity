<template>
  <v-app>
    <v-app-bar
      app
      color="#000E44"
      dark
    >
      <v-spacer></v-spacer>
      <div class="d-flex align-center justify-center">
        <v-img
          alt="singulARity Logo"
          class="shrink mr-2"
          contain
          src="https://i.imgur.com/lAKNvHN.png"
          transition="scale-transition"
          width="300"
        />
      </div>
      <v-spacer/>
    </v-app-bar>

    <v-main>
      <googleMap/>
    </v-main>
  </v-app>
</template>

<script>
import googleMap from './components/googleMap.vue';
// import userLocation from './components/userLocation';
export default {
  name: 'App',

  components: {
    googleMap,
    // userLocation,
  },

  data: () => ({
    //
  }),
};
</script>
