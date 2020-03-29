<template>
  <v-app>
    <div class="background">
    <v-app-bar
      app
      color="transparent"
      dark
      class="navbar"
      elevate-on-scroll
    >
      <div class="d-flex align-center">
        <v-img
          alt=""
          class="mr-2"
          contain
          src="./assets/logo.png"
          transition="scale-transition"
          width="200"
        />
      </div>

      <v-spacer></v-spacer>

    </v-app-bar>

    <v-content>

        <!-- Need to put this heading in separate component? Do v-containers in each individua component as allows for greater control over component backgrounds -->
        <v-container>
          <v-row>
            <div class="d-flex">
              <h1 class="home-title">Live Market Rates</h1>
              <v-icon large color="blue">mdi-chart-line</v-icon>
            </div>
          </v-row>
        </v-container>
          <Exchanger />
    </v-content>
    </div>
  </v-app>
</template>

<script>
import Exchanger from './components/Exchanger'


export default {
  name: 'App',

  components: {
    Exchanger
  },

  data: () => ({
    //
  }),
};
</script>

<style scoped>
.navbar {
  position: fixed;
}
.background {
  background-color: #1938a8;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100%25' height='100%25' viewBox='0 0 2 1'%3E%3Cdefs%3E%3ClinearGradient id='a' gradientUnits='userSpaceOnUse' x1='0' x2='0' y1='0' y2='1'%3E%3Cstop offset='0' stop-color='%231938a8'/%3E%3Cstop offset='1' stop-color='%23589bb0'/%3E%3C/linearGradient%3E%3ClinearGradient id='b' gradientUnits='userSpaceOnUse' x1='0' y1='0' x2='0' y2='1'%3E%3Cstop offset='0' stop-color='%23c9e2ff' stop-opacity='0'/%3E%3Cstop offset='1' stop-color='%23c9e2ff' stop-opacity='1'/%3E%3C/linearGradient%3E%3ClinearGradient id='c' gradientUnits='userSpaceOnUse' x1='0' y1='0' x2='2' y2='2'%3E%3Cstop offset='0' stop-color='%23c9e2ff' stop-opacity='0'/%3E%3Cstop offset='1' stop-color='%23c9e2ff' stop-opacity='1'/%3E%3C/linearGradient%3E%3C/defs%3E%3Crect x='0' y='0' fill='url(%23a)' width='2' height='1'/%3E%3Cg fill-opacity='0.76'%3E%3Cpolygon fill='url(%23b)' points='0 1 0 0 2 0'/%3E%3Cpolygon fill='url(%23c)' points='2 1 2 0 0 0'/%3E%3C/g%3E%3C/svg%3E");
  background-attachment: fixed;
  background-size: cover;
}
.home-title {
  margin: 20px 10px 20px 0px;
  font-size: 2.5em;
  color: white;
}
</style>