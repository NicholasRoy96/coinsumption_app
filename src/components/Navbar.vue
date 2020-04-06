<template>
    <v-app-bar
    app
    dark
    color="#002272"
    class="navbar"
    >
        <v-container>
            <div class="d-flex">
                <h1>coin</h1><h1 class="sumption">sumption</h1>
                <v-icon large class="logo">mdi-currency-usd-circle-outline</v-icon>
            </div>
        </v-container>
    </v-app-bar>
</template>

<script>
export default {
    name: 'Navbar'
}
</script>

<style scoped>
.sumption {
    color: #b5e0e8;
}
.logo {
    padding: 3px 0px 0px 5px;
}
</style>