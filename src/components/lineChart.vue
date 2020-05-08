<template>
    <v-row>
        <v-col cols="12">
            <highcharts :options="opt"></highcharts>
        </v-col>
    </v-row>
</template>

<script>
    import {Chart} from 'highcharts-vue'
    export default {
        name: 'lineChart',
        props: ['arrCases','arrDailyCases','arrDailyActiveCases','arrDeaths','arrRecovers','arrActiveCases'],
        data: () =>({
            highcharts: Chart,
            opt:  ''
        }),
        created() {
            this.getvals();
        },
        methods: {
            getvals(){
                this.opt = {
                    chart: {
                        type: 'spline',
                        backgroundColor: (this.$vuetify.theme.dark) && '#121212',
                    },
                    title: {
                        text: ' '
                    },
                    legend: {
                        itemStyle: {
                            color: (this.$vuetify.theme.dark) && '#ededed',
                        }
                    },
                    xAxis: {
                        title: {
                            text: 'عدد الأيام منذ أول حالة',
                            style: {
                                color: (this.$vuetify.theme.dark) && '#ededed',
                            }
                        },
                    },
                    yAxis: {
                        title: {
                            text: 'عدد الحالات',
                            style: {
                                color: (this.$vuetify.theme.dark) && '#ededed',
                            }
                        },
                        stackLabels: {
                            enabled: true,
                        }
                    },
                    plotOptions: {
                        series: {
                            dataLabels: {
                                enabled: true,
                                style: {
                                    color: (this.$vuetify.theme.dark) && '#ededed',
                                }
                            },
                            marker: {
                              enabled: false,
                            },
                        }
                    },
                    series: [
                    {
                        name: 'الحالات اليومية',
                        color: '#1E88E5',
                        data: this.arrDailyCases
                    },
                    {
                        name: 'الحالات النشطة اليومية',
                        type: 'column',
                        borderColor: 'transparent',
                        color: '#f9a825',
                        data: this.arrDailyActiveCases
                    },
                    {
                        name: 'الحالات المؤكدة',
                        color: '#546e7a',
                        visible: false,
                        data: this.arrCases
                    },
                    {
                        name: 'الحالات النشطة',
                        color: '#f9a825',
                        data: this.arrActiveCases
                    },
                    {
                        name: 'الوفيات',
                        color: '#EF5350',
                        data: this.arrDeaths
                    },
                    {
                        name: 'المتعافون',
                        color: '#81C784',
                        data: this.arrRecovers
                    },
                    // {
                    //     name: 'الوفيات اليومية',
                    //     color: '#EF5350',
                    //     data: this.arrDailyDeaths
                    // },
                    // {
                    //     name: 'المتعافون اليوميين',
                    //     color: '#81C784',
                    //     data: this.arrDailyRecovers
                    // },
                    ]
                };
            }
        }
    }
</script>
