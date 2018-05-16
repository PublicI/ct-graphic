<template>
    <div>
        <div id="chart"></div>
        
        <p class="source">Source: Center for Public Integrity analysis of Internal Revenue Service documents.</p>
    </div>
</template>

<script>
/* eslint-disable */

import data from '~/assets/indexed-means.json';
import Highcharts from 'highcharts';

export default {
    methods: {
        makeSeries(mydata) {
            const tar = [];
            for (let i = 0; i < mydata.index.length; i++) {
                let seriesI = {};
                seriesI.name = mydata.index[i];
                seriesI.data = mydata.data[i];
                tar.push(seriesI);
            }
            return tar;
        }
    },
    mounted() {
        let dSeries = this.makeSeries(data);

        Highcharts.chart('chart', {
            colors: ['red', '#ff6100', '#ff9900', 'rgb(150,150,150)'],
            title: {
                display: 'null',
                text: ' ',
                x: -20 // center
            },
            subtitle: {
                display: 'null',
                text: ' ',
                x: -20
            },
            credits: {
                enabled: false
            },
            xAxis: {
                labels: {
                    style: {
                        fontSize: '10px'
                    }
                },
                categories: data.columns
            },
            yAxis: {
                title: {
                    text: '% Change in income since 2007'
                },
                labels: {
                    formatter() {
                        return `${Highcharts.numberFormat(this.value, 0)}%`;
                    }
                    // step: 2
                },
                plotLines: [
                    {
                        value: 0,
                        width: 1,
                        color: '#808080'
                    }
                ]
            },
            tooltip: {
                valuePrefix: '$'
            },
            legend: {
                enabled: true,
                layout: 'vertical',
                align: 'right',
                verticalAlign: 'middle',
                borderWidth: 0
            },
            series: dSeries
        });
    }
};
</script>

<style scoped>
#chart {
    height: 400px;
    margin: 0 auto;
    padding-bottom: 20px;
}
.source {
    font-size:80%;
    color:#666;
}
</style>
