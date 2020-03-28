<template>
    <div>
    
    <v-card>
        <v-container>

            <!-- Input Row -->
            <v-row>

                <!-- From Currency Input Value -->
                <v-col cols="12" md="3">
                    <v-text-field label="Amount" v-model="inputConversionAmount" type="number" outlined>
                    </v-text-field>
                </v-col>

                <!-- From Currency Selector -->

                <v-col cols="12" md="3">
                    <v-select :items="currencies" item-text="compiled" item-value="abbr" label="From" v-model="inputFromCurrency" outlined>
                    </v-select>
                </v-col>

                <!-- Swap Currency Arrow -->
                <v-col cols="1" class="d-none d-md-flex">
                    <v-icon v-on:click="swapCurrency">mdi-swap-horizontal</v-icon>
                </v-col>

                <!-- To Currency Selector -->
                <v-col cols="12" md="3">
                    <v-select :items="currencies" item-text="compiled" item-value="abbr" label="To" v-model="inputToCurrency" outlined>
                    </v-select>
                </v-col>

                <!-- Submit Button -->
                <v-col cols="12" md="2" align-self="center">
                    <v-btn class="align-center" block v-on:click="exchangeCurrency">Go</v-btn>
                </v-col>
            </v-row>

            <!-- Output Row -->
            <v-row>
               <v-col cols="12" class="text-center">
                   <h1 v-if="exchangeVisible">{{this.outputConversionAmount}} {{this.outputFromCurrency}} = {{this.resultAmount}} {{this.outputToCurrency}}</h1>
                   <h2 v-if="singleExchangeVisible">1 {{this.outputFromCurrency}} = {{this.exchangeRate}} {{this.outputToCurrency}}</h2>
                   <h2 v-if="exchangeVisible"> 1 {{this.outputToCurrency}} = {{this.reverseExchangeRate}} {{this.outputFromCurrency}}</h2>
                   <h3 v-if="exchangeVisible" class="last-updated">Last Updated:  {{this.updatedDate}}</h3>
                   <v-progress-circular v-if="loadingCircleVisible" indeterminate color="primary"></v-progress-circular>
               </v-col> 
            </v-row>
        </v-container>
    </v-card>

    <v-row v-if="exchangeVisible" class="mt-8">
        <v-col cols="6">
            <ExchangerTable :exchangeDetails="exchangeDetails"/>
        </v-col>
        <v-col cols="6">
            <ExchangerTable :exchangeDetails="reverseExchangeDetails"/>
        </v-col>
    </v-row>
    </div>
</template>

<script>
import axios from 'axios'
import ExchangerTable from './ExchangerTable'

export default {
    name: 'Exchanger',
    components: {
        ExchangerTable
    },
    data () {
        return {
            currencies: [
                {abbr: 'SGD', name: 'Singapore Dollar', compiled: 'SGD - Singapore Dollar', flag: require('../assets/ukIcon.svg') },
                {abbr: 'MYR', name: 'Malaysian Ringgit', compiled: 'MYR - Malaysian Ringgit', flag: require('../assets/malaysiaIcon.svg')},
                {abbr: 'EUR', name: 'Euro', compiled: 'EUR - Euro', flag: require('../assets/euroIcon.svg')},
                {abbr: 'USD', name: 'US Dollar', compiled: 'USD - US Dollar', flag: require('../assets/usIcon.svg')},
                {abbr: 'AUD', name: 'Australian Dollar', compiled: 'AUD - Australian Dollar', flag: require('../assets/australiaIcon.svg')},
                {abbr: 'JPY', name: 'Japanese Yen', compiled: 'JPY - Japanese Yen', flag: require('../assets/japanIcon.svg')},
                {abbr: 'CNH', name: 'Chinese Yuan Renminbi', compiled: 'CNH - Chinese Yuan Renminbi', flag: require('../assets/chinaIcon.svg')},
                {abbr: 'HKD', name: 'Hong Kong Dollar', compiled: 'HKD - Hong Kong Dollar', flag: require('../assets/hkIcon.svg')},
                {abbr: 'CAD', name: 'Canadian Dollar', compiled: 'CAD - Canadian Dollar', flag: require('../assets/canadaIcon.svg')},
                {abbr: 'INR', name: 'Indian Rupee', compiled: 'INR - Indian Rupee', flag: require('../assets/indiaIcon.svg')},
                {abbr: 'DKK', name: 'Danish Krone', compiled: 'DKK - Danish Krone', flag: require('../assets/denmarkIcon.svg')},
                {abbr: 'GBP', name: 'British Pound', compiled: 'GBP - British Pound', flag: require('../assets/ukIcon.svg')},
                {abbr: 'RUB', name: 'Russian Ruble', compiled: 'RUB - Russian Ruble', flag: require('../assets/russiaIcon.svg')},
                {abbr: 'NZD', name: 'New Zealand Dollar', compiled: 'NZD - New Zealand Dollar', flag: require('../assets/nzIcon.svg')},
                {abbr: 'MXN', name: 'Mexican Peso', compiled: 'MXN - Mexican Peso', flag: require('../assets/mexicoIcon.svg')},
                {abbr: 'IDR', name: 'Indonesian Rupiah', compiled: 'IDR - Indonesian Rupiah', flag: require('../assets/indonesiaIcon.svg')},
                {abbr: 'TWD', name: 'Taiwan New Dollar', compiled: 'TWD - Taiwan New Dollar', flag: require('../assets/taiwanIcon.svg')},
                {abbr: 'THB', name: 'Thai Baht', compiled: 'THB - Thai Baht', flag: require('../assets/thailandIcon.svg')},
                {abbr: 'VND', name: 'Vietnamese Dong', compiled: 'VND - Vietnamese Dong', flag: require('../assets/vietnamIcon.svg')}
            ],
            inputConversionAmount: 1,
            inputFromCurrency: 'GBP',
            inputToCurrency: 'EUR',
            outputConversionAmount: 0,
            outputFromCurrency: '',
            outputToCurrency: '',
            resultAmount: 0,
            exchangeRate: 0,
            reverseExchangeRate: 0,
            exchangeVisible: false,
            singleExchangeVisible: false,
            loadingCircleVisible: false,
            updatedDate: '',
            error: ''
        }
    },
    methods: {
        swapCurrency() {
            const from = this.inputFromCurrency
            const to = this.inputToCurrency
            this.inputFromCurrency = to
            this.inputToCurrency = from
        },
        async exchangeCurrency() {
            this.exchangeVisible = false
            this.singleExchangeVisible = false
            this.loadingCircleVisible = true
            const exchangeOriginal = {
                "method":"GET",
                "url":"https://currency-exchange.p.rapidapi.com/exchange",
                "headers":{
                "content-type":"application/octet-stream",
                "x-rapidapi-host":"currency-exchange.p.rapidapi.com",
                "x-rapidapi-key":"db4802aa4dmsh6affbbbbbdfd496p18a2cbjsna51f8f9ae89d"
                },"params":{
                "from":this.inputFromCurrency,
                "to":this.inputToCurrency
                }
            }
            const exchangeReverse = {
                "method":"GET",
                "url":"https://currency-exchange.p.rapidapi.com/exchange",
                "headers":{
                "content-type":"application/octet-stream",
                "x-rapidapi-host":"currency-exchange.p.rapidapi.com",
                "x-rapidapi-key":"db4802aa4dmsh6affbbbbbdfd496p18a2cbjsna51f8f9ae89d"
                },"params":{
                "from":this.inputToCurrency,
                "to":this.inputFromCurrency
                }
            }
            axios.all([axios(exchangeOriginal), axios(exchangeReverse)])
            .then(axios.spread((...responses) => {
                this.loadingCircleVisible = false
                this.outputConversionAmount = this.inputConversionAmount
                this.outputFromCurrency = this.inputFromCurrency
                this.outputToCurrency = this.inputToCurrency
                this.exchangeRate = responses[0].data
                this.reverseExchangeRate = responses[1].data
                this.resultAmount = this.outputConversionAmount * this.exchangeRate
                this.exchangeVisible = true
                if (this.outputConversionAmount !== 1) this.singleExchangeVisible = true
                this.updatedDate = new Date().toUTCString()
            }))
            .catch((error) => {
                console.log(error)
            })
        }
    },
    computed: {
        exchangeDetails() {
            return {
                exchangeRate: this.exchangeRate,
                toCurrencyDetails: this.currencies.filter(currency => currency.abbr === this.outputToCurrency).pop(),
                fromCurrencyDetails: this.currencies.filter(currency => currency.abbr === this.outputFromCurrency).pop()
            }
        },
        reverseExchangeDetails() {
            return {
                fromCurrency: this.outputToCurrency,
                toCurrency: this.outputFromCurrency,
                exchangeRate: this.reverseExchangeRate,
                toCurrencyDetails: this.currencies.filter(currency => currency.abbr === this.outputFromCurrency).pop(),
                fromCurrencyDetails: this.currencies.filter(currency => currency.abbr === this.outputToCurrency).pop()
            }
        }
    }
}
</script>

<style scoped>
.last-updated {
    margin-top: 10px;
    font-weight: normal;
    font-size: 1em;
}
</style>