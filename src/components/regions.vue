<template>
    <v-row class="text-center">
        <v-col cols="12">
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
</template>

<script>
    export default {
        name: 'regions',
        data: () => ({
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
            this.fetchCsv('https://raw.githubusercontent.com/aboullaite/Covid19-MA/master/stats/regions.csv');
        },
        methods: {
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
                        this.regions = jsonResult;
                    });
                });
            }
        }
    }
</script>
