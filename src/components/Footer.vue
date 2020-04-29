<template>
  <v-footer color="#002272" padless>
    <v-card flat tile color="transparent" class="lighten-1 white--text text-center" width="100%">
      <v-card-text>
        <v-btn
          class="mx-4 white--text"
          icon
          href="https://www.linkedin.com/in/nicholas-roy-b1078a19a/"
          target="_blank"
        >
          <v-icon size="24px">{{ linkedIn.icon }}</v-icon>
        </v-btn>
        <v-btn
          class="mx-4 white--text"
          icon
          href="https://github.com/NicholasRoy96"
          target="_blank"
        >
          <v-icon size="24px">{{ github.icon }}</v-icon>
        </v-btn>
      </v-card-text>

      <v-card-text class="white--text pt-0">
        Live exchange rates for this site provided by the
        <a
          class="footer-link"
          href="https://rapidapi.com/fyhao/api/currency-exchange"
          target="_blank"
        >Currency Exchange</a> open API
      </v-card-text>

      <v-divider></v-divider>

      <v-card-text class="white--text">
        {{ new Date().getFullYear() }} —
        <a class="footer-link" href="http://nicholasroy.co.uk/" target="_blank">
          <strong>Nicholas Roy</strong>
        </a>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
export default {
  name: "Footer",
  data() {
    return {
      linkedIn: {
        icon: "mdi-linkedin"
      },
      github: {
        icon: "mdi-github"
      }
    };
  }
};
</script>

<style>
.footer-link {
  color: white !important;
}
.footer-link:hover {
  color: lightgrey !important;
}
</style>