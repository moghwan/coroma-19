<template>
    <v-container>
        <appBar></appBar>

        <lineChart
                :arrCases="arrCases"
                :arrDeaths="arrDeaths"
                :arrRecovers="arrRecovers"></lineChart>

        <cardStates :diff="diff" :last="last"></cardStates>

        <regions></regions>

    </v-container>
</template>

<script>
    import appBar from '../views/header'
    import lineChart from './lineChart'
    import cardStates from './cardStates'
    import regions from './regions'

    export default {
        components: {
            appBar,
            lineChart,
            cardStates,
            regions,
        },
        data: () => ({
            arrCases: [],
            arrDeaths: [],
            arrRecovers: [],
            last: [],
            diff: [],
        }),

        mounted() {
            this.fetchCsv('https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/MA-times_series.csv');
        },

        created() {
            // this.$vuetify.theme.dark = true
        },
        methods: {
            reformatDate(date){
                const splited = date.split('/')
                return splited[2] + '-' + splited[1] + '-' + splited[0]
            },
            fetchCsv(url){
                fetch(url).then((response) => {
                    response.text().then((csvRegions) => {
                        let lines = csvRegions.split(/\r?\n/);
                        let fieldDelimiter = ',';
                        let headers = lines[0].split(fieldDelimiter);
                        let jsonResult = [];
                        for (let i = 1; i < lines.length; i++) {
                            let currentLine = lines[i].split(fieldDelimiter);
                            if (!currentLine[0]){ continue }
                            let jsonObject = {};
                            for (let j = 0; j < headers.length; j++) {
                                let propertyName = headers[j].replace(/\s/g, '');
                                jsonObject[propertyName] = currentLine[j];
                            }
                            jsonResult.push(jsonObject);
                        }

                        this.last = jsonResult[jsonResult.length - 1];
                        this.last['active'] = this.last['Cases/الحالات'] - this.last['Recovered/تعافى'] - this.last['Deaths/الوفيات'];

                        var beforeLast = jsonResult[jsonResult.length - 2];
                        beforeLast['active'] = beforeLast['Cases/الحالات'] - beforeLast['Recovered/تعافى'] - beforeLast['Deaths/الوفيات'];

                        this.diff['active'] = this.last['active'] - beforeLast['active'];
                        this.diff['Cases/الحالات'] = this.last['Cases/الحالات'] - beforeLast['Cases/الحالات'];
                        this.diff['Recovered/تعافى'] = this.last['Recovered/تعافى'] - beforeLast['Recovered/تعافى'];
                        this.diff['Deaths/الوفيات'] = this.last['Deaths/الوفيات'] - beforeLast['Deaths/الوفيات'];

                        var vm = this;

                        jsonResult.map(function (e) {
                            vm.arrCases.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Cases/الحالات'])]);
                            vm.arrDeaths.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Deaths/الوفيات'])]);
                            vm.arrRecovers.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Recovered/تعافى'])]);
                        });
                    });
                });
            }
        }
    }
</script>
