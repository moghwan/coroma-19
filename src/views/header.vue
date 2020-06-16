<template>
    <v-app-bar
            :color="!this.$vuetify.theme.dark ? 'white' : 'black'"
            app
            flat
            hide-on-scroll
    >
        <v-btn icon/>
        <v-btn icon/>
        <v-spacer></v-spacer>
        <v-toolbar-title>CORO<span class="ma">🇲🇦</span>-19</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn icon @click="switchDarkMode">
            <v-icon>mdi-invert-colors</v-icon>
        </v-btn>
        <v-btn icon href="https://github.com/moghwan/coroma-19">
            <v-icon>mdi-source-fork</v-icon>
        </v-btn>
    </v-app-bar>
</template>

<script>
    export default {
        name: 'appBar',
        methods: {
            switchDarkMode() {
                localStorage.setItem('isDark', !this.$vuetify.theme.dark);
                location.reload();
            }
        }
    }
</script>

<style>
    .ma {
        color: #ee534f ;
    }
</style>
