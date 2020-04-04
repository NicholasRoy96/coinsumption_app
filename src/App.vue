<template>
  <v-app>
    <div class="background">
    <v-app-bar
      app
      dark
      flat
      color="#244685"
      class="navbar"
    >
      <div class="d-flex align-center">
        <v-img
          alt=""
          class="logo"
          contain
          src="./assets/logo.png"
          transition="scale-transition"
        />
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-content>
        <Exchanger />
        <InfoCards />
    </v-content>
    </div>
  </v-app>
</template>

<script>
import Exchanger from './components/Exchanger'
import InfoCards from './components/InfoCards'


export default {
  name: 'App',

  components: {
    Exchanger,
    InfoCards
  },

  data: () => ({
    //
  }),
};
</script>

<style scoped>
.navbar {
  position: fixed;
  border-bottom: 1px solid #929db0 !important
}
.logo {
  width: 200px;
}
.background {
  background-color: rgb(0, 34, 114);
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100%25' height='100%25' viewBox='0 0 2 1'%3E%3Cdefs%3E%3ClinearGradient id='a' gradientUnits='userSpaceOnUse' x1='0' x2='0' y1='0' y2='1'%3E%3Cstop offset='0' stop-color='%231938a8'/%3E%3Cstop offset='1' stop-color='%23589bb0'/%3E%3C/linearGradient%3E%3ClinearGradient id='b' gradientUnits='userSpaceOnUse' x1='0' y1='0' x2='0' y2='1'%3E%3Cstop offset='0' stop-color='%23c9e2ff' stop-opacity='0'/%3E%3Cstop offset='1' stop-color='%23c9e2ff' stop-opacity='1'/%3E%3C/linearGradient%3E%3ClinearGradient id='c' gradientUnits='userSpaceOnUse' x1='0' y1='0' x2='2' y2='2'%3E%3Cstop offset='0' stop-color='%23c9e2ff' stop-opacity='0'/%3E%3Cstop offset='1' stop-color='%23c9e2ff' stop-opacity='1'/%3E%3C/linearGradient%3E%3C/defs%3E%3Crect x='0' y='0' fill='url(%23a)' width='2' height='1'/%3E%3Cg fill-opacity='0.76'%3E%3Cpolygon fill='url(%23b)' points='0 1 0 0 2 0'/%3E%3Cpolygon fill='url(%23c)' points='2 1 2 0 0 0'/%3E%3C/g%3E%3C/svg%3E");
  background-attachment: fixed;
  background-size: cover;
}
</style>