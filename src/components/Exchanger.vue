<template>
  <div class="background">
    <!-- Dynamic Titles -->
    <v-container class="pb-10">
      <div v-if="!exchangeVisible" class="home-titles-div">
        <h1 v-if="!exchangeVisible" class="home-title">Currency Converter</h1>
        <h1 class="home-subtitle">Live Market Rates</h1>
      </div>
      <div v-if="exchangeVisible" class="exchange-titles-div">
        <h1
          class="exchange-title"
        >{{this.outputConversionAmount}} {{this.outputFromCurrency}} to {{this.outputToCurrency}} = {{this.resultAmount}} {{this.outputToCurrencyName}}</h1>
      </div>
      <!-- Input Row -->
      <v-row class="pb-6 pl-6 pr-6">
        <!-- From Currency Input Value -->
        <v-col cols="12" md="3">
          <p class="input-label">Amount</p>
          <v-text-field
            background-color="white"
            class="input-value"
            v-model="inputConversionAmount"
            type="number"
            outlined
            rounded
            @click="clickAmount"
            v-on-clickaway="awayAmount"
          ></v-text-field>
        </v-col>

        <!-- From Currency Selector -->
        <v-col cols="12" md="3">
          <p class="input-label">From</p>
          <v-select
            background-color="white"
            :items="currencies"
            item-text="compiled"
            item-value="abbr"
            v-model="inputFromCurrency"
            outlined
            rounded
          ></v-select>
        </v-col>

        <!-- Swap Currency Arrow -->
        <v-col cols="1" class="d-none d-md-flex">
          <div class="icon-div">
            <v-icon
              class="swap-icon"
              x-large
              color="white"
              v-on:click="swapCurrency"
            >mdi-swap-horizontal</v-icon>
          </div>
        </v-col>

        <!-- To Currency Selector -->
        <v-col cols="12" md="3">
          <p class="input-label">To</p>
          <v-select
            background-color="white"
            :items="currencies"
            item-text="compiled"
            item-value="abbr"
            v-model="inputToCurrency"
            outlined
            rounded
          ></v-select>
        </v-col>

        <!-- Submit Button -->
        <!-- D-MD + -->
        <v-col cols="12" md="2" align-self="center">
          <v-btn
            class="submit-button d-none d-md-flex"
            color="rgb(252, 184, 19)"
            fab
            v-on:click="exchangeCurrency"
          >
            <v-icon size="70" color="white">mdi-chevron-right</v-icon>
          </v-btn>
          <!-- D-SM - -->
          <v-btn
            large
            class="submit-button d-md-none"
            color="rgb(252, 184, 19)"
            block
            rounded
            v-on:click="exchangeCurrency"
          >
            <v-icon large color="white">mdi-chevron-right</v-icon>
          </v-btn>
        </v-col>
      </v-row>

      <!-- Output Row -->
      <v-row>
        <v-col cols="12" class="text-center">
          <h1 v-if="exchangeVisible">
            {{this.outputConversionAmount}} {{this.outputFromCurrency}} =
            <h1 class="display-total">{{this.resultAmount}} {{this.outputToCurrency}}</h1>
          </h1>
          <h2 v-if="singleExchangeVisible">1 {{this.outputFromCurrency}} = {{this.exchangeRate}} {{this.outputToCurrency}}</h2>
          <h2 v-if="exchangeVisible">1 {{this.outputToCurrency}} = {{this.reverseExchangeRate}} {{this.outputFromCurrency}}</h2>
          <h3 v-if="exchangeVisible" class="last-updated">Last Updated: {{this.updatedDate}}</h3>
        </v-col>
      </v-row>
    </v-container>

    <!-- Exchange Table -->
    <div v-if="exchangeVisible" class="table-main-background">
      <v-container class="table-background">
        <v-row v-if="exchangeVisible" class="mt-8">
          <v-col cols="12" md="6">
            <ExchangerTable :exchangeDetails="exchangeDetails" />
          </v-col>
          <v-col cols="12" md="6">
            <ExchangerTable :exchangeDetails="reverseExchangeDetails" />
          </v-col>
        </v-row>
      </v-container>
    </div>

    <!-- Loading Overlay -->
    <v-overlay :value="loadingCircle">
      <v-progress-circular indeterminate size="64"></v-progress-circular>
    </v-overlay>

    <!-- Fallback Rates Snackbar -->
    <v-snackbar v-model="snackbar" :multi-line="multiLine" bottom right :timeout="timeout">
      {{ snackbarText }}
      <v-btn color="#b3c6e8" text @click="snackbar = false">Close</v-btn>
    </v-snackbar>
  </div>
</template>

<script>
import axios from "axios";
import ExchangerTable from "./ExchangerTable";
import { mixin as clickaway } from "vue-clickaway";
import fallbackJSON from "../assets/fallback";

export default {
  name: "Exchanger",
  components: {
    ExchangerTable
  },
  data() {
    return {
      inputConversionAmount: 1,
      inputFromCurrency: "GBP",
      inputToCurrency: "EUR",
      outputConversionAmount: 0,
      outputFromCurrency: "",
      outputToCurrency: "",
      outputToCurrencyName: "",
      resultAmount: 0,
      exchangeRate: 0,
      reverseExchangeRate: 0,
      exchangeVisible: false,
      singleExchangeVisible: false,
      loadingCircle: false,
      updatedDate: "",
      snackbar: false,
      multiLine: true,
      timeout: 0,
      snackbarText:
        "There seems to be an issue with the Open API being used for the currency exchange. For demo purposes, this app will use fallback exchange rates saved locally which may not currently be accurate.",
      error: false
    };
  },
  computed: {
    currencies() {
      return [
        {
          abbr: "SGD",
          name: "Singapore Dollars",
          compiled: "SGD - Singapore Dollar",
          flag: require("../assets/ukIcon.svg")
        },
        {
          abbr: "MYR",
          name: "Malaysian Ringgits",
          compiled: "MYR - Malaysian Ringgit",
          flag: require("../assets/malaysiaIcon.svg")
        },
        {
          abbr: "EUR",
          name: "Euros",
          compiled: "EUR - Euro",
          flag: require("../assets/euroIcon.svg")
        },
        {
          abbr: "USD",
          name: "US Dollars",
          compiled: "USD - US Dollar",
          flag: require("../assets/usIcon.svg")
        },
        {
          abbr: "AUD",
          name: "Australian Dollars",
          compiled: "AUD - Australian Dollar",
          flag: require("../assets/australiaIcon.svg")
        },
        {
          abbr: "JPY",
          name: "Japanese Yen",
          compiled: "JPY - Japanese Yen",
          flag: require("../assets/japanIcon.svg")
        },
        {
          abbr: "HKD",
          name: "Hong Kong Dollars",
          compiled: "HKD - Hong Kong Dollar",
          flag: require("../assets/hkIcon.svg")
        },
        {
          abbr: "CAD",
          name: "Canadian Dollars",
          compiled: "CAD - Canadian Dollar",
          flag: require("../assets/canadaIcon.svg")
        },
        {
          abbr: "INR",
          name: "Indian Rupees",
          compiled: "INR - Indian Rupee",
          flag: require("../assets/indiaIcon.svg")
        },
        {
          abbr: "DKK",
          name: "Danish Krone",
          compiled: "DKK - Danish Krone",
          flag: require("../assets/denmarkIcon.svg")
        },
        {
          abbr: "GBP",
          name: "British Pounds",
          compiled: "GBP - British Pound",
          flag: require("../assets/ukIcon.svg")
        },
        {
          abbr: "RUB",
          name: "Russian Rubles",
          compiled: "RUB - Russian Ruble",
          flag: require("../assets/russiaIcon.svg")
        },
        {
          abbr: "NZD",
          name: "New Zealand Dollars",
          compiled: "NZD - New Zealand Dollar",
          flag: require("../assets/nzIcon.svg")
        },
        {
          abbr: "MXN",
          name: "Mexican Pesos",
          compiled: "MXN - Mexican Peso",
          flag: require("../assets/mexicoIcon.svg")
        },
        {
          abbr: "IDR",
          name: "Indonesian Rupiah",
          compiled: "IDR - Indonesian Rupiah",
          flag: require("../assets/indonesiaIcon.svg")
        },
        {
          abbr: "TWD",
          name: "Taiwan New Dollars",
          compiled: "TWD - Taiwan New Dollar",
          flag: require("../assets/taiwanIcon.svg")
        },
        {
          abbr: "THB",
          name: "Thai Baht",
          compiled: "THB - Thai Baht",
          flag: require("../assets/thailandIcon.svg")
        },
        {
          abbr: "VND",
          name: "Vietnamese Dong",
          compiled: "VND - Vietnamese Dong",
          flag: require("../assets/vietnamIcon.svg")
        }
      ].sort((a, b) => a.abbr.localeCompare(b.abbr));
    },
    exchangeDetails() {
      return {
        exchangeRate: this.exchangeRate,
        toCurrencyDetails: this.currencies
          .filter(currency => currency.abbr === this.outputToCurrency)
          .pop(),
        fromCurrencyDetails: this.currencies
          .filter(currency => currency.abbr === this.outputFromCurrency)
          .pop()
      };
    },
    reverseExchangeDetails() {
      return {
        fromCurrency: this.outputToCurrency,
        toCurrency: this.outputFromCurrency,
        exchangeRate: this.reverseExchangeRate,
        toCurrencyDetails: this.currencies
          .filter(currency => currency.abbr === this.outputFromCurrency)
          .pop(),
        fromCurrencyDetails: this.currencies
          .filter(currency => currency.abbr === this.outputToCurrency)
          .pop()
      };
    }
  },
  mixins: [clickaway],
  methods: {
    clickAmount() {
      this.inputConversionAmount = "";
    },
    awayAmount() {
      if (this.inputConversionAmount === "") this.inputConversionAmount = 1;
      const amount = parseInt(this.inputConversionAmount);
      if (amount === 0) this.inputConversionAmount = 1;
      if (amount < 0) this.inputConversionAmount = amount * -1;
    },
    swapCurrency() {
      const from = this.inputFromCurrency;
      const to = this.inputToCurrency;
      this.inputFromCurrency = to;
      this.inputToCurrency = from;
    },
    async exchangeCurrency() {
      this.error = false;
      this.loadingCircle = true;
      const exchangeOriginal = {
        method: "GET",
        url: "https://currency-exchange.p.rapidapi.com/exchange",
        headers: {
          "content-type": "application/octet-stream",
          "x-rapidapi-host": "currency-exchange.p.rapidapi.com",
          "x-rapidapi-key": "db4802aa4dmsh6affbbbbbdfd496p18a2cbjsna51f8f9ae89d"
        },
        params: {
          from: this.inputFromCurrency,
          to: this.inputToCurrency
        }
      };
      const exchangeReverse = {
        method: "GET",
        url: "https://currency-exchange.p.rapidapi.com/exchange",
        headers: {
          "content-type": "application/octet-stream",
          "x-rapidapi-host": "currency-exchange.p.rapidapi.com",
          "x-rapidapi-key": "db4802aa4dmsh6affbbbbbdfd496p18a2cbjsna51f8f9ae89d"
        },
        params: {
          from: this.inputToCurrency,
          to: this.inputFromCurrency
        }
      };
      axios
        .all([axios(exchangeOriginal), axios(exchangeReverse)])
        .then(
          axios.spread((...responses) => {
            if (typeof responses[0].data !== Number) throw new Error
            this.exchangeRate = responses[0].data;
            this.reverseExchangeRate = responses[1].data;
            this.outputConversionAmount = this.inputConversionAmount;
            this.outputFromCurrency = this.inputFromCurrency;
            this.outputToCurrency = this.inputToCurrency;
            const currencyObject = this.currencies
              .filter(currency => currency.abbr === this.outputToCurrency)
              .pop();
            this.outputToCurrencyName = currencyObject.name;
            
            this.resultAmount = this.outputConversionAmount * this.exchangeRate;
            if (this.resultAmount === 0) throw new Error;
            if (this.resultAmount <= 9) {
              this.resultAmount = this.resultAmount.toFixed(5);
            }
            if (this.resultAmount > 9 && this.resultAmount <= 49) {
              this.resultAmount = this.resultAmount.toFixed(4);
            }
            if (this.resultAmount > 49 && this.resultAmount <= 999) {
              this.resultAmount = this.resultAmount.toFixed(3);
            }
            if (this.resultAmount > 999) {
              this.resultAmount = this.resultAmount.toFixed(2);
            }
            this.loadingCircle = false;
            this.exchangeVisible = true;
            if (this.outputConversionAmount !== 1)
              this.singleExchangeVisible = true;
            this.updatedDate = new Date().toUTCString();
          })
        )
        .catch(error => {
          console.log(error);
          this.error = true;
          this.fallbackExchange();
        });
    },
    async fallbackExchange() {
      this.outputConversionAmount = this.inputConversionAmount;
      this.outputFromCurrency = this.inputFromCurrency;
      this.outputToCurrency = this.inputToCurrency;
      const currencyObject = this.currencies
        .filter(currency => currency.abbr === this.outputToCurrency)
        .pop();
      this.outputToCurrencyName = currencyObject.name;
      this.exchangeRate =
        fallbackJSON[this.outputFromCurrency][this.outputToCurrency];
      this.reverseExchangeRate =
        fallbackJSON[this.outputToCurrency][this.outputFromCurrency];
      console.log(this.exchangeRate)
      this.resultAmount = this.outputConversionAmount * this.exchangeRate;
      if (this.resultAmount <= 9) {
        this.resultAmount = this.resultAmount.toFixed(5);
      }
      if (this.resultAmount > 9 && this.resultAmount <= 49) {
        this.resultAmount = this.resultAmount.toFixed(4);
      }
      if (this.resultAmount > 49 && this.resultAmount <= 999) {
        this.resultAmount = this.resultAmount.toFixed(3);
      }
      if (this.resultAmount > 999) {
        this.resultAmount = this.resultAmount.toFixed(2);
      }
      this.loadingCircle = false;
      this.snackbar = true;
      this.exchangeVisible = true;
      if (this.outputConversionAmount !== 1) this.singleExchangeVisible = true;
      this.updatedDate = new Date().toLocaleString();
    }
  }
};
</script>

<style scoped>
.home-title {
  margin: 20px 10px 5px 0px;
  font-size: 2.5em;
  color: white;
}
.home-subtitle {
  margin: 0px 10px 20px 0px;
  font-size: 2em;
  color: white;
  font-style: italic;
}
.exchange-title {
  margin: 20px 10px 20px 0px;
  font-size: 2em;
  color: white;
}
.background {
  background-color: transparent;
  color: white;
}
.input-label {
  padding: 0px 0px 6px 4px;
  margin: 0;
  font-weight: bold;
}
.icon-div {
  margin: auto;
  width: 50%;
}
.submit-button {
  color: rgb(252, 184, 19);
}
.last-updated {
  margin-top: 10px;
  font-weight: normal;
  font-size: 1em;
}
.table-main-background {
  background: rgb(231, 244, 254);
  border-top: 5px solid rgb(252, 184, 19);
}
.table-background {
  background: white;
}
.display-total {
  font-size: 2.8em;
}

/* MEDIA QUERIES */

/* MD */
@media (max-width: 960px) {
  .display-total {
    font-size: 2em;
  }
  .home-titles-div {
    margin-left: 22px;
  }
  .home-title {
    font-size: 2em;
  }
  .home-subtitle {
    font-size: 1.5em;
  }
  .exchange-titles-div {
    margin-left: 20px;
  }
  .exchange-title {
    font-size: 1.7em;
  }
}

/* SM */
@media (max-width: 600px) {
  .display-total {
    font-size: 1.4em;
  }
  .home-title {
    font-size: 1.8em;
  }
  .home-subtitle {
    font-size: 1.3em;
  }
  .exchange-title {
    font-size: 1.2em;
  }
  .last-updated {
    font-size: 0.9em;
  }
}
</style>