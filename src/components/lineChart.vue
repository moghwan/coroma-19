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
        props: [
            'arrCases','arrDailyCases','arrDailyActiveCases','arrDeaths','arrRecovers','arrActiveCases','arrDailyRecovers','arrDailyDeaths',
            'showDataLabels',
            'showXyTitles',
            'showLegendDailyCases',
            'showLegendDailyActiveCases',
            'showLegendDailyRecovers',
            'showLegendDailyDeaths',
            'showLegendCases',
            'showLegendActiveCases',
            'showLegendRecovers',
            'showLegendDeaths',
        ],
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
                        type: 'area',
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
                            text: (this.showXyTitles) && 'عدد الأيام منذ أول حالة',
                            style: {
                                color: (this.$vuetify.theme.dark) && '#ededed',
                            }
                        },
                    },
                    yAxis: {
                        title: {
                            text: (this.showXyTitles) && 'عدد الحالات',
                            style: {
                                color: (this.$vuetify.theme.dark) && '#ededed',
                            }
                        },
                    },
                    tooltip: {
                        shared: true,
                        useHTML:true,
                        headerFormat: '<small>{point.key}</small><table>',
                        pointFormat: "<tr>" +
                            "<td style='color: {series.color};text-align: right'><b>{point.y:,.0f}</b></td>" +
                            "<td style='text-align: right'>: {series.name}</td></tr>",
                        footerFormat: '</table>',
                    },
                    plotOptions: {
                        series: {
                            dataLabels: {
                                enabled: (!!this.showDataLabels),
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
                        borderColor: 'transparent',
                        color: '#1E88E5',
                        data: this.arrDailyCases,
                        showInLegend: (!!this.showLegendDailyCases)
                    },
                    {
                        name: 'الحالات النشطة اليومية',
                        borderColor: 'transparent',
                        color: '#f9a825',
                        data: this.arrDailyActiveCases,
                        showInLegend: (!!this.showLegendDailyActiveCases)
                    },
                    {
                        name: 'الحالات المؤكدة',
                        color: '#546e7a',
                        //visible: false,
                        data: this.arrCases,
                        showInLegend: (!!this.showLegendCases)
                    },
                    {
                        name: 'المتعافون',
                        color: '#81C784',
                        data: this.arrRecovers,
                        showInLegend: (!!this.showLegendRecovers)
                    },
                    {
                        name: 'الحالات النشطة',
                        color: '#f9a825',
                        data: this.arrActiveCases,
                        showInLegend: (!!this.showLegendActiveCases)
                    },
                    {
                        name: 'الوفيات',
                        color: '#EF5350',
                        data: this.arrDeaths,
                        showInLegend: (!!this.showLegendDeaths)
                    },
                    {
                        name: 'الوفيات اليومية',
                        borderColor: 'transparent',
                        color: '#EF5350',
                        data: this.arrDailyDeaths,
                        showInLegend: (!!this.showLegendDailyDeaths)
                    },
                    {
                        name: 'المتعافون اليوميين',
                        borderColor: 'transparent',
                        color: '#81C784',
                        data: this.arrDailyRecovers,
                        showInLegend: (!!this.showLegendDailyRecovers)
                    },
                    ]
                };
            }
        }
    }
</script>
