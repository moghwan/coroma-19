<template>
  <v-container>
    <v-row class="text-center">
        <v-col cols="6">
          <v-alert
                  border="left"
                  outlined
                  type="info"
                  elevation="2"
          >
            {{ info_regions }}.
          </v-alert>
          <v-data-table
                  :headers="regionsHeader"
                  :items="regions"
                  :itemsPerPage=20
                  class="elevation-1">
          </v-data-table>
        </v-col>
        <v-col cols="6">
          <v-alert
                  border="left"
                  outlined
                  type="info"
                  elevation="2"
          >
            {{ info_timeSeries }}.
          </v-alert>
          <v-data-table
                  :headers="timesSeriesHeader"
                  :items="timesSeries"
                  :itemsPerPage=20
                  class="elevation-1">
          </v-data-table>
        </v-col>
        <v-col cols="12">
          <v-alert
                  border="left"
                  outlined
                  type="info"
                  elevation="2"
          >
            {{ info_cities }}.
          </v-alert>
          <v-data-table
                  :headers="citiesHeader"
                  :items="cities"
                  :itemsPerPage=20
                  class="elevation-1">
          </v-data-table>
        </v-col>
</v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',

    data: () => ({
      info_cities: 'COVID-19 Data in Morocco by city / بيانات فيروس كورونا في المغرب حسب المدينة',
      info_timeSeries: 'Time series of COVID-19 data in Morocco / التسلسل الزمني لبيانات فيروس',
      info_regions: 'COVID-19 Data n Morocco by region / بيانات فيروس كورونا في المغرب حسب الجهة',
      cities: [],
      citiesHeader: [
        { text: 'City/المدينة:', value: 'City/المدينة', },
        { text: 'Region/المنطقة', value: 'Region/المنطقة' },
        { text: 'ActiveCases/الحالاتالنشطة', value: 'ActiveCases/الحالاتالنشطة' },
        { text: 'TotalDeaths/إجماليالوفيات', value: 'TotalDeaths/إجماليالوفيات' },
        { text: 'TotalRecovered/إجماليالمعافين', value: 'TotalRecovered/إجماليالمعافين' },
      ],
      timesSeries: [],
      timesSeriesHeader: [
        { text: 'Dates/التواريخ:', value: 'Dates/التواريخ', },
        { text: 'Cases/الحالات', value: 'Cases/الحالات' },
        { text: 'Recovered/تعافى', value: 'Recovered/تعافى' },
        { text: 'Deaths/الوفيات', value: 'Deaths/الوفيات' },
      ],
      regions: [],
      regionsHeader: [
        {
          text: 'Region/الجهة',
          align: 'start',
          sortable: false,
          value: 'Region/الجهة',
        },
        { text: 'TotalCases/إجماليالحالات', value: 'TotalCases/إجماليالحالات' },
        { text: 'ActiveCases/الحالاتالنشطة', value: 'ActiveCases/الحالاتالنشطة' },
        { text: 'TotalDeaths/إجماليالوفيات', value: 'TotalDeaths/إجماليالوفيات' },
        { text: 'TotalRecovered/إجماليالمعافين', value: 'TotalRecovered/إجماليالمعافين' },
      ],
    }),

    mounted() {
    },

    created() {
      this.fetchCsv('c','https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/cities.csv');
      this.fetchCsv('ts', 'https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/MA-times_series.csv');
      this.fetchCsv('r', 'https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/regions.csv');
    },
    methods: {
      fetchCsv(val, url){
        fetch(url).then((response) => {
          response.text().then((csvRegions) => {
            let lines = csvRegions.split(/\r?\n/);
            let fieldDelimiter = ',';
            let headers = lines[0].split(fieldDelimiter);
console.log(headers);
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
              case 'c':
                this.cities = jsonResult;
                console.log(jsonResult);
                break;
              case 'ts':
                this.timesSeries = jsonResult;
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
