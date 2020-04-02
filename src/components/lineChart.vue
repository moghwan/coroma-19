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
        props: ['arrCases','arrDeaths','arrRecovers'],
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
                        type: 'spline'
                    },
                    title: {
                        text: ' '
                    },
                    xAxis: {
                        title: {
                            text: 'عدد الأيام منذ أول حالة'
                        },
                    },
                    yAxis: {
                        title: {
                            text: 'عدد الحالات'
                        },
                    },
                    plotOptions: {
                        series: {
                            dataLabels: {
                                enabled: true
                            }
                        }
                    },
                    series: [{
                        name: 'الحالات المؤكدة',
                        color: '#546e7a',
                        data: this.arrCases
                    },
                    {
                        name: 'الوفيات',
                        type: 'column',
                        color: '#EF5350',
                        data: this.arrDeaths
                    },
                    {
                        name: 'المتعافون',
                        color: '#81C784',
                        data: this.arrRecovers
                    }]
                };
            }
        }
    }
</script>
