<template>
    <div>
        <div id="chart"></div>
        
        <p class="source">Source: Center for Public Integrity analysis of American Community Survey data. Graphic by Rosie Cima.</p>
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
                seriesI.pointStart = Date.UTC(2007,0,1); 
                seriesI.pointIntervalUnit = 'year'
                seriesI.marker = { symbol: 'circle', radius: 3.5 };
                tar.push(seriesI);
            }
            return tar;
        }
    },
    mounted() {
        let cols = this.makeDates(data.columns)
        let dSeries = this.makeSeries(data);

        Highcharts.setOptions({
            chart: {
                style: {
                    fontFamily: 'tablet-gothic-narrow'
                }
            }
        });



        Highcharts.chart('chart', {
            colors: ['#cbcbcb', '#979797', '#f46d43','#af2020'],
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
                    color: 'rgb(243, 243, 243)',
                    from: Date.UTC(2007,11,1),
                    to: Date.UTC(2009,6,1),
                    label: {
                        text: 'Recession',
                        align: 'center',
                        style: {
                            color: '#737373'
                        }
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
                    text: 'Change in mean household income since 2007'
                },
                labels: {
                    formatter() {
                        return `${Highcharts.numberFormat(this.value, 0)}%`;
                    }
                },
                plotLines: [
                    {
                        value: 0,
                        width: 1.5,
                        color: '#bebebe'
                    }
                ]
            },
            tooltip: {
                pointFormat: '{series.name}:<b>{point.y:.1f}%</b>',
                crosshairs: true
            },
            legend: {
                enabled: true,
                align: 'right',
                verticalAlign: 'bottom',
                borderWidth: 0
            },
            series: dSeries,
            title: {
                text: '<b>Bridgeport-Stamford-Norwalk metro area recovered from the recession slowly, unequally</b>'
            },
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
    line-height:120%;
    font-size:80%;
    color:#666;
}
</style>
