<template>
    <div class="wheather">
        <h1>{{ msg }}</h1>
        <button v-on:click="getWeatherData">Get Weather Data</button>
        <grid :cols="cols" :rows="weatherDataList" :pagination="true" :search="true" :sort="true" v-if="weatherDataList"></grid>
        <LineChart v-if="loaded" :chartdata="chartdata" :options="options" />
    </div>
</template>

<script>
    import LineChart from './LineChart.vue';
    import Grid from 'gridjs-vue'

    export default {
        name: 'WeatherForecast',

        components: {
            LineChart,
            Grid
        },

        props: {
            msg: String
        },

        data: () => ({
            cols: ['Date', 'Temperature C', 'Temperature F', 'Summary'],
            loaded: false,
            chartdata: null,
            options: {
                responsive: true,
                maintainAspectRatio: false
            },
            weatherDataList: null
        }),

        methods: {
            async getWeatherData() {
                this.loaded = false;

                await fetch("http://localhost:8080/weatherforecast/")
                    .then(response => response.json())
                    .then(data => (this.weatherDataList = data));

                this.chartdata = {
                    labels: this.weatherDataList.map(x => x.Date),
                    datasets: [
                        {
                            label: 'Data One',
                            backgroundColor: '#f87979',
                            data: this.weatherDataList.map(x => x.temperatureC)
                        }
                    ]
                };
                this.loaded = true;
            }
        }
    }
</script>
