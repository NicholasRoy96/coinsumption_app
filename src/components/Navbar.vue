<template>
  <v-app-bar app dark color="#002272" class="navbar">
    <v-container>
      <a href="/" class="heading-link"> 
      <div class="d-flex">
        <h1>coin</h1>
        <h1 class="sumption">sumption</h1>
        <v-icon large class="logo">mdi-currency-usd-circle-outline</v-icon>
      </div>
      </a>
    </v-container>
  </v-app-bar>
</template>

<script>
export default {
  name: "Navbar"
};
</script>

<style scoped>
.heading-link {
  text-decoration: none;
}
.sumption {
  color: #b5e0e8;
}
.logo {
  padding: 3px 0px 0px 5px;
}
</style>