<template>
    <v-row class="text-center">
        <v-col cols="12" lg="3" sm="6">
            <v-card class="mx-auto">
                <v-card-text class="headline font-weight-bold blue-grey darken-1">
                    <p class="grey--text text--lighten-5">
                        الحالات المؤكدة
                    </p>
                    <v-progress-circular
                            indeterminate
                            color="white"
                            v-if="!this.last['Cases/الحالات']"
                    ></v-progress-circular>
                    <p class="grey--text text--lighten-5" v-else>
                        <v-badge
                                :content="this.diff['Cases/الحالات']"
                                :value="this.diff['Cases/الحالات']"
                                color="blue darken-1"
                        >
                            {{ this.last['Cases/الحالات'] }}
                        </v-badge>
                    </p>
                </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12" lg="3" sm="6">
            <v-card class="mx-auto">
                <v-card-text class="headline font-weight-bold yellow darken-3">
                    <p class="grey--text text--lighten-5">
                        الحالات النشطة
                    </p>
                    <v-progress-circular
                            indeterminate
                            color="white"
                            v-if="!this.last['active']"
                    ></v-progress-circular>
                    <p class="grey--text text--lighten-5" v-else>
                        <v-badge
                                :content="this.diff['active']"
                                :value="this.diff['active']"
                                color="blue darken-1"
                        >
                            {{ this.last['active'] }}
                        </v-badge>
                    </p>
                </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12" lg="3" sm="6">
            <v-card class="mx-auto">
                <v-card-text class="headline font-weight-bold red lighten-1">
                    <p class="grey--text text--lighten-5">
                        الوفيات
                    </p>
                    <v-progress-circular
                            indeterminate
                            color="white"
                            v-if="!this.last['Deaths/الوفيات']"
                    ></v-progress-circular>
                    <p class="grey--text text--lighten-5" v-else>
                        <v-badge
                                :content="this.diff['Deaths/الوفيات']"
                                :value="this.diff['Deaths/الوفيات']"
                                color="blue darken-1"
                        >
                            {{ this.last['Deaths/الوفيات'] }}
                        </v-badge>
                    </p>
                </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12" lg="3" sm="6">
            <v-card class="mx-auto" color="80c783">
                <v-card-text class="headline font-weight-bold green lighten-2">
                    <p class="grey--text text--lighten-5">
                        المتعافون
                    </p>
                    <v-progress-circular
                            indeterminate
                            color="white"
                            v-if="!this.last['Recovered/تعافى']"
                    ></v-progress-circular>
                    <p class="grey--text text--lighten-5" v-else>
                        <v-badge
                                :content="this.diff['Recovered/تعافى']"
                                :value="this.diff['Recovered/تعافى']"
                                color="blue darken-1"
                        >
                            {{ this.last['Recovered/تعافى'] }}
                        </v-badge>
                    </p>
                </v-card-text>
            </v-card>
        </v-col>
    </v-row>
</template>

<script>
    export default {
        name: 'cardStates',
        props: ['last', 'diff'],
    }
</script>
