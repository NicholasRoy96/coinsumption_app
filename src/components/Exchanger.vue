<template>
    
    <v-card>
        <v-container>
            <v-row>
                <v-col cols="3">
                    <v-text-field label="Amount" v-model="inputConversionAmount" type="number">
                    </v-text-field>
                </v-col>
                <v-col cols="3">
                    <v-select :items="currencies" label="From" v-model="inputFromCurrency">
                    </v-select>
                </v-col>
                <v-col cols="1" align-self="center" class="text-center">
                    <v-icon v-on:click="swapCurrency">mdi-swap-horizontal</v-icon>
                </v-col>
                <v-col cols="3">
                    <v-select :items="currencies" label="To" v-model="inputToCurrency">
                    </v-select>
                </v-col>
                <v-col cols="2" align-self="center">
                    <v-btn class="align-center" block v-on:click="exchangeCurrency">Go</v-btn>
                </v-col>
            </v-row>
            <v-row>
               <v-col cols="12" class="text-center">
                   <h1 v-if="exchangeVisible">{{this.outputConversionAmount}} {{this.outputFromCurrency}} = {{this.resultAmount}} {{this.outputToCurrency}}</h1>
                   <h2 v-if="exchangeVisible">1 {{this.outputFromCurrency}} = {{this.exchangeRate}} {{this.outputToCurrency}}</h2>
                   <v-progress-circular v-if="loadingCircleVisible" indeterminate color="primary"></v-progress-circular>
               </v-col> 
            </v-row>
        </v-container>
    </v-card>

    
    
</template>

<script>
import axios from 'axios'

export default {
    name: 'Exchanger',
    data () {
        return {
            currencies: [],
            inputConversionAmount: 1,
            inputFromCurrency: 'GBP',
            inputToCurrency: 'EUR',
            outputConversionAmount: 0,
            outputFromCurrency: '',
            outputToCurrency: '',
            resultAmount: 0,
            exchangeRate: 0,
            exchangeVisible: false,
            loadingCircleVisible: false,
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
            this.loadingCircleVisible = true
            axios({
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
            })
            .then((response) => {
                this.loadingCircleVisible = false
                this.outputConversionAmount = this.inputConversionAmount
                this.outputFromCurrency = this.inputFromCurrency
                this.outputToCurrency = this.inputToCurrency
                this.exchangeRate = response.data
                this.resultAmount = this.outputConversionAmount * this.exchangeRate
                this.exchangeVisible = true
            })
            .catch((error) => {
                console.log(error)
            })
        },
        async getCurrencies() {
            await axios({
                "method":"GET",
                "url":"https://currency-exchange.p.rapidapi.com/listquotes",
                "headers":{
                "content-type":"application/octet-stream",
                "x-rapidapi-host":"currency-exchange.p.rapidapi.com",
                "x-rapidapi-key":"db4802aa4dmsh6affbbbbbdfd496p18a2cbjsna51f8f9ae89d"
                }
            })
            .then((response)=>{
                this.currencies = response.data
            })
            .catch((error)=>{
                console.log(error)
            })
        }
    },
    async mounted () {
        await this.getCurrencies()
    }
}
</script>

<style>

</style>