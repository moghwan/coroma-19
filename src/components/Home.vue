<template>
    <v-container>
        <appBar></appBar>

        <lineChart
                :arrCases="arrCases"
                :arrActiveCases="arrActiveCases"
                :arrDeaths="arrDeaths"
                :arrRecovers="arrRecovers"></lineChart>

        <v-row class="text-center">
            <v-col cols="6" lg="3" sm="6">
                <cardStates :color="'blue-grey darken-1'" :label="'الحالات المؤكدة'" :diff="diff['Cases/الحالات']" :last="last['Cases/الحالات']"></cardStates>
            </v-col>
            <v-col cols="6" lg="3" sm="6">
                <cardStates :color="'yellow darken-3'" :label="'الحالات النشيطة'" :diff="diff['active']" :last="last['active']"></cardStates>
            </v-col>
            <v-col cols="6" lg="3" sm="6">
                <cardStates :color="'red lighten-1'" :label="'الوفيات'" :diff="diff['Deaths/الوفيات']" :last="last['Deaths/الوفيات']"></cardStates>
            </v-col>
            <v-col cols="6" lg="3" sm="6">
                <cardStates :color="'green lighten-2'" :label="'المتعافون'" :diff="diff['Recovered/تعافى']" :last="last['Recovered/تعافى']"></cardStates>
            </v-col>
        </v-row>
        <lineChart
                :arrDailyCases="arrDailyCases"
                :arrDailyActiveCases="arrDailyActiveCases"
        ></lineChart>

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
            arrDailyCases: [],
            arrDailyActiveCases: [],
            arrDailyDeaths: [],
            arrDailyRecovers: [],
            arrCases: [],
            arrActiveCases: [],
            arrDeaths: [],
            arrRecovers: [],
            last: [],
            diff: [],
        }),

        mounted() {
            this.fetchCsv('https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/MA-times_series.csv');
        },
        created() {
            this.$vuetify.theme.dark = (localStorage.getItem('isDark') !== 'false');
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

                        const vm = this;

                        jsonResult.forEach((e,i) =>{
                            let newCase;
                            let newDeaths;
                            let newRecovers;
                            let newActiveCase;
                            if (i) {
                                newCase = e['Cases/الحالات'] - jsonResult[i-1]['Cases/الحالات'];
                                newDeaths = e['Deaths/الوفيات'] - jsonResult[i-1]['Deaths/الوفيات'];
                                newRecovers = e['Recovered/تعافى'] - jsonResult[i-1]['Recovered/تعافى'];
                                newActiveCase =
                                    (e['Cases/الحالات'] - e['Deaths/الوفيات'] - e['Recovered/تعافى'])
                                        -
                                    (jsonResult[i-1]['Cases/الحالات'] - jsonResult[i-1]['Deaths/الوفيات'] - jsonResult[i-1]['Recovered/تعافى'])
                                ;
                            } else {
                                newActiveCase = newCase = 1;
                            }

                            vm.arrDailyCases.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(newCase)]);
                            vm.arrDailyDeaths.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(newDeaths)]);
                            vm.arrDailyRecovers.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(newRecovers)]);
                            vm.arrDailyActiveCases.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(newActiveCase)]);
                            vm.arrCases.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Cases/الحالات'])]);
                            vm.arrActiveCases.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Cases/الحالات'] - e['Deaths/الوفيات'] - e['Recovered/تعافى'])]);
                            vm.arrDeaths.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Deaths/الوفيات'])]);
                            vm.arrRecovers.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Recovered/تعافى'])]);
                        });

                        console.log(vm.arrDailyCases);
                        console.log(vm.arrCases);

                    });
                });
            }
        }
    }
</script>
