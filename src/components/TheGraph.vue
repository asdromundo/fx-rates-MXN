<template>
    <div id="home">
        <div class="chart1">
            <div id="lineChart"></div>
        </div>
        {{info}}
    </div>
</template>
<script>

// Consuming API
// var endpoint = 'latest';
// var access_key = '';

// define from currency, to currency, and amount
var from = 'MXN';
var to = 'USD';

var start_date = '2022-03-01';
var end_date = '2022-08-01'
var date_range = (start_date === "" ? 'latest?' : start_date + ".." + end_date + "?");

import c3 from 'c3';

export default {
    //name: TheGraph,
    data() {
        return {
            data: {},
            info: {}
        }
    },
    beforeMount() {
        //this.getFXFromFrankfurter();
    },
    mounted() {
        this.chartAPI();
    },
    methods: {
        chart() {
            let chart = c3.generate({
                bindto: '#lineChart',
                data: {
                    columns: [
                        ['data1', 30, 200, 100, 400, 150, 250],
                        ['data2', 50, 20, 10, 40, 15, 25]
                    ],
                    axes: {
                        data1: 'y',
                        data2: 'y2',
                    }
                },
                axis: {
                    x: {
                        label: 'X Label'
                    },
                    y: {
                        label: {
                            text: 'Y Axis Label',
                            position: 'outer-middle'
                        }
                    },
                    y2: {
                        show: true,
                        label: {
                            text: 'Y2 Axis Label',
                            position: 'outer-middle'
                        }
                    }
                },
                tooltip: {
                    //  enabled: false
                },
                zoom: {
                    // enabled: true
                },
                subchart: {
                    // show: false
                }
            });

            setTimeout(function () {
                chart.axis.labels({
                    x: 'New X Axis Label',
                    y: 'New Y Axis Label',
                    y2: 'New Y2 Axis Label',
                });
            }, 1000);

            setTimeout(function () {
                chart.load({
                    columns: [
                        ['data1', 100, 300, 600, 200, 400, 500]
                    ]
                });
                chart.axis.labels({ y: 'New Y Axis Label Again' });
            }, 2000);
        },
        async getFXFromFrankfurter() {
            // execute the conversion using the "convert" endpoint:
            const url = 'https://api.frankfurter.app/' + date_range + 'amount=1&from=' + from + "&to=" + to;
            const res = await fetch(url);
            const data = await res.json();
            this.info = data;

        },
        chartAPI() {
            this.getFXFromFrankfurter();
            console.log(this.data != this.info);
            let chart = c3.generate({
                bindto: '#lineChart',
                data: {
                    url: this.info,
                   
                }
            });
        } 
    }
}
</script>
<style>
@import "https://cdnjs.cloudflare.com/ajax/libs/c3/0.7.0/c3.min.css";
/*
    #lineChart .c3-line-data2 {
       stroke-width: 5px; 
    }
    #lineChart .c3-axis-x line,
    #lineChart .c3-axis-x path {
        stroke:rgb(255, 255, 255);
    }
    */

#lineChart .c3-axis path,
line {
    stroke: rgb(205, 210, 214);
}
</style>