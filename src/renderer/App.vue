<template>
    <v-app id="inspire" dark>
        <v-content>
            <router-view></router-view>
        </v-content>
        <v-footer app fixed class="pl-2">
            <span>&copy; 2018 Component Team</span>
        </v-footer>
    </v-app>
</template>

<script>
    export default {
        name: 'dashboard'
    }
</script>

<style>
    html{
        overflow-y: hidden!important;
    }
</style>
