<template>
    <v-container>
        <v-row class="text-center">
            <v-col cols="12" lg="3" sm="6">
              <v-card class="mx-auto">
                  <v-card-text class="headline font-weight-bold">
                      <p>Confirmed</p>
                      <p class="blue--text">{{ this.counts['Cases/الحالات'] }}</p>
                  </v-card-text>
              </v-card>
            </v-col>
            <v-col cols="12" lg="3" sm="6">
              <v-card class="mx-auto">
                  <v-card-text class="headline font-weight-bold">
                      <p>Active Cases</p>
                      <p class="orange--text">{{ this.counts['active'] }}</p>
                  </v-card-text>
              </v-card>
            </v-col>
            <v-col cols="12" lg="3" sm="6">
              <v-card class="mx-auto">
                  <v-card-text class="headline font-weight-bold">
                      <p>Deaths</p>
                      <p class="red--text">{{ this.counts['Deaths/الوفيات'] }}</p>
                  </v-card-text>
              </v-card>
            </v-col>
            <v-col cols="12" lg="3" sm="6">
                <v-card class="mx-auto">
                    <v-card-text class="headline font-weight-bold">
                        <p>Recovered</p>
                        <p class="light-green--text">{{ this.counts['Recovered/تعافى'] }}</p>
                    </v-card-text>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12">
                <highcharts :options="data"></highcharts>
            </v-col>
        </v-row>
        <v-row class="text-center">
            <v-col cols="12" lg="6" sm="12" xs="12"><v-alert
                        border="left"
                        outlined
                        type="info"
                        elevation="2"
                >
                    التسلسل الزمني لبيانات فيروس كورونا
                </v-alert>
                <v-data-table
                        :headers="timesSeriesHeader"
                        :items="timesSeries"
                        :itemsPerPage=20
                        :sortDesc=true
                        sortBy="Dates/التواريخ"
                        class="elevation-1">
                </v-data-table>
            </v-col>
            <v-col cols="12" lg="6" sm="12" xs="12">
              <v-alert
                      border="left"
                      outlined
                      type="info"
                      elevation="2"
              >
                  بيانات فيروس كورونا في المغرب حسب الجهة
              </v-alert>
              <v-data-table
                      :headers="regionsHeader"
                      :items="regions"
                      :itemsPerPage=20
                      :sortDesc=true
                      sortBy="TotalCases/إجماليالحالات"
                      class="elevation-1">
              </v-data-table>
          </v-col>
      </v-row>
  </v-container>
</template>

<script>
import {Chart} from 'highcharts-vue'
export default {
    data: () => ({
      highcharts: Chart,
      data: [],
      counts: [],
      timesSeriesChart: [],
      timesSeries: [],
      timesSeriesHeader: [
        { text: 'Dates/التواريخ:', value: 'Dates/التواريخ', },
        { text: 'Cases/الحالات', value: 'Cases/الحالات' },
        { text: 'Recovered/تعافى', value: 'Recovered/تعافى' },
        { text: 'Deaths/الوفيات', value: 'Deaths/الوفيات' },
      ],
      regions: [],
      regionsHeader: [
        { text: 'Region/الجهة', value: 'Region/الجهة', },
        { text: 'TotalCases/إجمالي الحالات', value: 'TotalCases/إجماليالحالات' },
        { text: 'ActiveCases/الحالات النشطة', value: 'ActiveCases/الحالاتالنشطة' },
        { text: 'TotalDeaths/إجمالي الوفيات', value: 'TotalDeaths/إجماليالوفيات' },
        { text: 'TotalRecovered/إجمالي المعافين', value: 'TotalRecovered/إجماليالمعافين' },
      ],
    }),

    mounted() {
        this.fetchCsv('ts', 'https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/MA-times_series.csv');
        this.fetchCsv('r', 'https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/regions.csv');
    },

    created() {
    },
    methods: {
      fetchCsv(val, url){
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

            switch (val) {
              case 'ts':
                this.timesSeries = jsonResult;
                this.counts = jsonResult[jsonResult.length - 1];
                this.counts['active'] = this.counts['Cases/الحالات'] - this.counts['Recovered/تعافى'] - this.counts['Deaths/الوفيات'];
                this.data =  {
                  series: [{
                      data: jsonResult.map(function (e) {
                          return parseInt(e['Cases/الحالات']);
                      })
                  }]
              };
                  break;
              case 'r':
                this.regions = jsonResult;
                break;
              default:
                console.log('first param is absent');
            }
          });
        });
      }
    }
  }
</script>
