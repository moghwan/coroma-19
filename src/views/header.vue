<template>
    <v-app-bar
            :color="!this.$vuetify.theme.dark ? 'white' : 'black'"
            app
            flat
            hide-on-scroll
    >
        <v-spacer></v-spacer>
        <v-toolbar-title>CORO<span class="ma">🇲🇦</span>-19</v-toolbar-title>
        <v-btn icon @click="switchDarkMode">
            <v-icon>mdi-invert-colors</v-icon>
        </v-btn>
        <v-spacer></v-spacer>
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
