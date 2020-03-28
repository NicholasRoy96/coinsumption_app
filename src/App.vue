<template>
  <v-app>
    <v-app-bar
      app
      color="black"
      dark
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
      <v-container>
        <h1 class="home-title text-center">Coinsumption Converter</h1>
        <Exchanger />
      </v-container>
    </v-content>
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
.home-title {
  margin: 20px 0px 20px 0px;
  font-size: 2.5em;
  color: black;
}
</style>