<template>
    <v-card :loading="loading" align="center" height="100%">
        <v-card-title class="justify-center pb-0">Light</v-card-title>
        <apexchart
            type="area"
            height="280px"
            :options="chartOptions"
            ref="lightChart"
            :series="series"
        />
    </v-card>
</template>

<script lang="ts">
/* eslint-disable */
import { Vue } from 'vue-property-decorator';
import { Light } from '../../types';

export default Vue.extend({
    props: {
        initialData: {
            type: Array,
        },
    },
    data() {
        return {
            loading: false,
            lightData: [],
            timestamps: [],
            series: [
                {
                    name: 'Light',
                    data: [],
                },
            ],
            chartOptions: {
                chart: {
                    type: 'area',
                    stacked: false,
                    height: 350,
                    zoom: {
                        enabled: false,
                    },
                },
                dataLabels: {
                    enabled: false,
                },
                markers: {
                    size: 0,
                },
                fill: {
                    type: 'gradient',
                    gradient: {
                        shadeIntensity: 1,
                        inverseColors: false,
                        opacityFrom: 0.5,
                        opacityTo: 0,
                        stops: [0, 90, 100],
                    },
                },
                yaxis: {
                    min: 0,
                    max: 200000,
                    labels: {
                        formatter: (val: any) => val.toFixed(0),
                    },
                    title: {
                        text: 'Percentage',
                    },
                },
                xaxis: {
                    title: {
                        text: 'Time',
                    },
                },
                tooltip: {
                    shared: false,
                    y: {
                        formatter: (val: any) => val.toFixed(0),
                    },
                },
            },
        };
    },

    mounted() {
        this.initialData.forEach((element: any) => {
            // @ts-ignore
            this.lightData.push(element.value);
            const [t, _] = new Date(element.timestamp).toTimeString().split(' ');
            // @ts-ignore
            this.timestamps.push(t);
        });
        this.updateChart();
    },

    methods: {
        updateChart() {
            // @ts-ignore
            this.$refs.lightChart.updateSeries([
                {
                    data: this.$data.lightData,
                },
            ]);

            // @ts-ignore
            this.$refs.lightChart.updateOptions({
                xaxis: {
                    categories: this.$data.timestamps,
                },
            });
        },
    },
    watch: {
        /* eslint-disable */
        lightData: function() {
            this.updateChart();
        },
    },

    sockets: {
        light(data: string) {
            const message: Light = JSON.parse(data);
            const value = message.value.toFixed(2);
            const [timestamp, _] = new Date(message.at).toTimeString().split(' ');

            if (this.$data.lightData.length > 9) {
                this.$data.lightData.shift();
                this.$data.timestamps.shift();
            }

            this.$data.lightData.push(value);
            this.$data.timestamps.push(timestamp);
        },
    },
});
</script>
