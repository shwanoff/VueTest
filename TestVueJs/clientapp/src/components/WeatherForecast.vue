<template>
    <div class="wheather">
        <h1>{{ msg }}</h1>
        <button v-on:click="getWeatherData">Get Weather Data</button>
        <LineChart v-if="loaded" :chartdata="chartdata" :options="options" />
        <div class="table-responsive">
            <table class="table-hover" v-if="weatherDataList">
                <thead>
                    <tr>
                        <th>Id</th>
                        <th>Date</th>
                        <th>Temperature C</th>
                        <th>Temperature F</th>
                        <th>Summary</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, idx) in weatherDataList" :key="idx">
                        <td>{{ idx }}</td>
                        <td>{{ item.date }}</td>
                        <td>{{ item.temperatureC }}</td>
                        <td>{{ item.temperatureF }}</td>
                        <td>{{ item.summary }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
    import LineChart from './LineChart.vue';

    export default {
        name: 'WeatherForecast',

        components: {
            LineChart
        },

        props: {
            msg: String
        },

        data: () => ({
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
