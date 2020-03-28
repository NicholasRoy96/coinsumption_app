<template>
    <div>
        <v-row class="mb-4" justify="center">
            <v-col cols="10" class="text-center">
                <h1 class="conversion-title">Convert {{this.exchangeDetails.fromCurrencyDetails.name}} to {{this.exchangeDetails.toCurrencyDetails.name}}</h1>
            </v-col>
        </v-row>

        <v-row class="d-flex">
            <v-col cols="6">
                <div class="flag-container">
                    <v-img class="flag" :src='this.exchangeDetails.fromCurrencyDetails.flag' transition="scale-transition"/>
                </div>
            </v-col>
            <v-col cols="6">
                <div class="flag-container">
                    <v-img class="flag" :src='this.exchangeDetails.toCurrencyDetails.flag' transition="scale-transition"/>
                </div>
            </v-col>
        </v-row>
        
        <v-data-table
            :headers="headers"
            :items="values"
            hide-default-footer
            class="mt-4"
        ></v-data-table>
    </div>
</template>

<script>
export default {
    name: "ExchangerTable",
    props: {
        exchangeDetails: {
            type: Object
        }
    },
    data () {
        return {
            exchangeValues: {
                one: 0,
                five: 0,
                ten: 0,
                twentyfive: 0,
                fifty: 0,
                onehundred: 0,
                fivehundred: 0,
                onethousand: 0,
                fivethousand: 0,
                tenthousand: 0,
                fiftythousand: 0
            }
        }
    },
    computed: {
        headers() {
             return [
                {text: this.exchangeDetails.fromCurrencyDetails.abbr, value: 'fromValue', sortable: false},
                {text: this.exchangeDetails.toCurrencyDetails.abbr, value: 'toValue', sortable: false}
            ]
        },
        values() {
            const { fromCurrencyDetails, toCurrencyDetails } = this.exchangeDetails
            return [
                {fromValue: `1 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.one.toFixed(5)} ${toCurrencyDetails.abbr}`},
                {fromValue: `5 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.five.toFixed(5)} ${toCurrencyDetails.abbr}`},
                {fromValue: `10 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.ten.toFixed(4)} ${toCurrencyDetails.abbr}`},
                {fromValue: `25 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.twentyfive.toFixed(4)} ${toCurrencyDetails.abbr}`},
                {fromValue: `50 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.fifty.toFixed(4)} ${toCurrencyDetails.abbr}`},
                {fromValue: `100 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.onehundred.toFixed(3)} ${toCurrencyDetails.abbr}`},
                {fromValue: `500 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.fivehundred.toFixed(3)} ${toCurrencyDetails.abbr}`},
                {fromValue: `1000 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.onethousand.toFixed(3)} ${toCurrencyDetails.abbr}`},
                {fromValue: `5000 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.fivethousand.toFixed(3)} ${toCurrencyDetails.abbr}`},
                {fromValue: `10000 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.tenthousand.toFixed(2)} ${toCurrencyDetails.abbr}`},
                {fromValue: `50000 ${fromCurrencyDetails.abbr}`, toValue: `${this.exchangeValues.fiftythousand.toFixed(2)} ${toCurrencyDetails.abbr}`}
            ]
        }
    },
    methods: {
        calculateExchanges() {
            this.exchangeValues.one = this.exchangeDetails.exchangeRate
            this.exchangeValues.five = this.exchangeDetails.exchangeRate * 5
            this.exchangeValues.ten = this.exchangeDetails.exchangeRate * 10
            this.exchangeValues.twentyfive = this.exchangeDetails.exchangeRate * 25
            this.exchangeValues.fifty = this.exchangeDetails.exchangeRate * 50
            this.exchangeValues.onehundred = this.exchangeDetails.exchangeRate * 100
            this.exchangeValues.fivehundred = this.exchangeDetails.exchangeRate * 500
            this.exchangeValues.onethousand = this.exchangeDetails.exchangeRate * 1000
            this.exchangeValues.fivethousand = this.exchangeDetails.exchangeRate * 5000
            this.exchangeValues.tenthousand = this.exchangeDetails.exchangeRate * 10000
            this.exchangeValues.fiftythousand = this.exchangeDetails.exchangeRate * 50000
        }
    },
    watch: {
        'exchangeDetails.exchangeRate': function () {
        this.calculateExchanges()
        }
    },
    mounted () {
        this.calculateExchanges()
    }
}
</script>

<style scoped>
.flag-container {
    display: flex;
    justify-content: center;
}

.flag {
    max-width: 30%;
}

.conversion-title {
    font-size: 1.3em;
    letter-spacing: 1px;
    color: #1f3c5e;
}
</style>