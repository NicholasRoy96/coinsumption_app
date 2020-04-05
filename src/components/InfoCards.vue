<template>
    <div class="background">
        <v-container>
            <v-row justify="center">
                <v-col cols="12" md="6" lg="5">
                    <v-container class="card-one">
                        <div align="center">
                            <v-icon color="#002272" size="100">{{cardOne.icon}}</v-icon>
                        </div>
                        <h1 class="card-heading">{{cardOne.heading}}</h1>
                        <ul class="card-list">
                            <li class="card-item" v-for="item in cardOne.list" :key="item">{{item}}</li>
                        </ul>
                        <div align="right" class="card-button-div">
                            <v-btn outlined class="card-button">MORE INFORMATION</v-btn>
                        </div>
                    </v-container>
                </v-col>

                <v-col cols="12" md="6" lg="5">
                    <v-container class="card-two">
                        <div align="center">
                            <v-icon color="#002272" size="100">{{cardTwo.icon}}</v-icon>
                        </div>
                        <h1 class="card-heading">{{cardTwo.heading}}</h1>
                        <ul>
                            <li class="card-item" v-for="item in cardTwo.list" :key="item">{{item}}</li>
                        </ul>
                        <div align="right" class="card-button-div">
                            <v-btn outlined class="card-button">MORE INFORMATION</v-btn>
                        </div>
                    </v-container>
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>

<script>
export default {
    name: "InfoCards",
    data () {
        return {
            cardOne: {icon: "mdi-cash-usd-outline", heading: "International money transfers", list: ["Quick, easy, secure", "Great rates", "Access 24/7"]},
            cardTwo: {icon: "mdi-bell-alert-outline", heading: "Rate alerts", list: ["Choose a currency pair", "Set your desired mid-market rate", "Receive free email alerts"]}
        }
    }
}
</script>

<style scoped>
.background {
    background: #F8F8F8 !important;
    border-top: 5px solid rgb(0, 34, 114);
}
.card-one {
    padding: 40px 28px 40px 0px;
    border-right: 1px solid lightgrey;
}
.card-two {
    padding: 40px 0px 40px 28px;
}
.card-heading {
    margin: 30px 0px;
    color: #002272;
    font: 24px "Noto Sans", sans-serif;
    font-weight: 700;
    letter-spacing: 0.7px;
    line-height: 1.3;
}
ul {
  list-style: none;
}

ul li::before {
  content: "\2713";
  color: #abd2ff;
  font-weight: bold;
  display: inline-block;
  width: 2em;
  margin-left: -1em;
}
.card-item {
    padding-bottom: 15px;
    color: #434341;
}
.card-button {
    color: #005CEF;
}
.card-button:hover {
    background: #005CEF;
    color: #F8F8F8;
}
.card-button-div {
    margin-top: 40px;
}

/* MEDIA QUERIES */

/* MD */
@media (max-width: 960px) {
    .card-one {
        border-right: none;
        border-bottom: 1px solid lightgrey;
        padding: 40px;
    }
    .card-two {
        padding: 40px;
    }
}

</style>