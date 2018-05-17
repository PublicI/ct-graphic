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
        makeDates(floatArr) {
            let tar = [];
            for (let i = 0; i < floatArr.length; i++) {
                tar[i] = Date.UTC(floatArr[i],0,1);
            }
            return tar;
        },
        makeSeries(mydata) {
            const tar = [];
            for (let i = 0; i < mydata.index.length; i++) {
                let seriesI = {};
                seriesI.name = mydata.index[i];
                seriesI.data = mydata.data[i];
                // Doing this to compensate for 2 leap years and it's fine
                // but this kind of thing would never happen in D3 just saying
                seriesI.pointStart = Date.UTC(2007,0,3); 
                seriesI.pointInterval = 365 * 24 * 3600 * 1000
                seriesI.marker = { symbol: 'circle' };
                tar.push(seriesI);
            }
            return tar;
        }
    },
    mounted() {
        let cols = this.makeDates(data.columns)
        let dSeries = this.makeSeries(data);
        Highcharts.chart('chart', {
            colors: ['#525252', '#969696','#bd0026', '#f03b20'],
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
                plotBands: [{ 
                    color: '#eee0cd',
                    from: Date.UTC(2007,11,1),
                    to: Date.UTC(2009,6,1),
                    label: {
                        text: 'Recession',
                        align: 'right',
                        x: -10
                    }
                }],
                labels: {
                    style: {
                        fontSize: '10px'
                    }
                },
                type: 'datetime'
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
            series: dSeries,
            style: {
                fontFamily: 'tablet-gothic-narrow'
            }
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
