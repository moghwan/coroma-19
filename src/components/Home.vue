<template>
    <v-container>
        <v-app-bar
                color="white"
                app
                flat
                hide-on-scroll
        >
            <v-spacer></v-spacer>
            <v-toolbar-title>CORO<span class="ma">🇲🇦</span>-19</v-toolbar-title>
            <v-spacer></v-spacer>
        </v-app-bar>

        <v-row>
            <v-col cols="12">
                <highcharts :options="data"></highcharts>
            </v-col>
        </v-row>
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
        <v-row class="text-center">
            <v-col cols="12" lg="6" sm="12" xs="12"><v-alert
                        color="blue-grey"
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
                        sortBy="Cases/الحالات"
                        class="elevation-1">
                </v-data-table>
            </v-col>
            <v-col cols="12" lg="6" sm="12" xs="12">
              <v-alert
                      color="blue-grey"
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

<style scoped lang="scss">

    $body-font-family: "C" !important;
    .main {
        background-color: #62b7fa;
    }
    .ma {
        color: #ee534f ;
    }
</style>

<script>
import {Chart} from 'highcharts-vue'
export default {
    data: () => ({
      highcharts: Chart,
      data: [],
      last: [],
      diff: [],
      timesSeriesChart: [],
      timesSeries: [],
      timesSeriesHeader: [
        { text: 'التواريخ:', value: 'Dates/التواريخ', },
        { text: 'الحالات', value: 'Cases/الحالات' },
        { text: 'تعافى', value: 'Recovered/تعافى' },
        { text: 'الوفيات', value: 'Deaths/الوفيات' },
      ],
      regions: [],
      regionsHeader: [
        { text: 'الجهة', value: 'Region/الجهة', },
        { text: 'إجمالي الحالات', value: 'TotalCases/إجماليالحالات' },
        { text: 'الحالات النشطة', value: 'ActiveCases/الحالاتالنشطة' },
        { text: 'إجمالي الوفيات', value: 'TotalDeaths/إجماليالوفيات' },
        { text: 'إجمالي المعافين', value: 'TotalRecovered/إجماليالمعافين' },
      ],
    }),

    mounted() {
        this.fetchCsv('ts', 'https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/MA-times_series.csv');
        this.fetchCsv('r', 'https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/regions.csv');
    },

    created() {
        // this.$vuetify.theme.dark = true
    },
    methods: {
        reformatDate(date){
            const splited = date.split('/')
            return splited[2] + '-' + splited[1] + '-' + splited[0]
        },
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
                this.last = jsonResult[jsonResult.length - 1];
                this.last['active'] = this.last['Cases/الحالات'] - this.last['Recovered/تعافى'] - this.last['Deaths/الوفيات'];
                var beforeLast = jsonResult[jsonResult.length - 2];
                beforeLast['active'] = beforeLast['Cases/الحالات'] - beforeLast['Recovered/تعافى'] - beforeLast['Deaths/الوفيات'];
                this.diff['active'] = this.last['active'] - beforeLast['active'];
                this.diff['Cases/الحالات'] = this.last['Cases/الحالات'] - beforeLast['Cases/الحالات'];
                this.diff['Recovered/تعافى'] = this.last['Recovered/تعافى'] - beforeLast['Recovered/تعافى'];
                this.diff['Deaths/الوفيات'] = this.last['Deaths/الوفيات'] - beforeLast['Deaths/الوفيات'];

                var vm = this;
                var arrCases = [];
                var arrDeaths = [];
                var arrRecovers = [];
                jsonResult.map(function (e) {
                    arrCases.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Cases/الحالات'])]);
                    arrDeaths.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Deaths/الوفيات'])]);
                    arrRecovers.push([vm.reformatDate(e['Dates/التواريخ']),parseInt(e['Recovered/تعافى'])]);
                });

                this.data =  {
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
                        data: arrCases
                    },
                    {
                        name: 'الوفيات',
                        type: 'column',
                        color: '#EF5350',
                        data: arrDeaths
                    },
                    {
                        name: 'المتعافون',
                        color: '#81C784',
                        data: arrRecovers
                    }
                ]
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
