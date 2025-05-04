<template>
    <v-card class="mx-auto">
        <v-card-text class="headline font-weight-bold" :class="color">
            <p class="grey--text text--lighten-5">
                {{label}}
            </p>
            <v-progress-circular
                    indeterminate
                    color="white"
                    v-if="!this.last"
            ></v-progress-circular>
            <p class="grey--text text--lighten-5" v-else>
                <v-badge
                        :content="this.diff"
                        :value="this.diff"
                        color="blue darken-1"
                >
                    {{ this.last }}
                </v-badge>
                <br>
                <small v-if="this.percentage">({{percentage}}%)</small>
                <small v-else>(100%)</small>
            </p>
        </v-card-text>
    </v-card>
</template>

<script>
    export default {
        name: 'cardStates',
        props: ['last', 'diff', 'color', 'label', 'percentage'],
    }
</script>
