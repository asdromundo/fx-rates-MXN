<template>
    <div id="home">
        <div class="chart1">
            <div id="lineChart"></div>
        </div>
            </div>
</template>
<script>

// Consuming API
// var endpoint = 'latest';
// var access_key = '';

// define from currency, to currency, and amount
var from = 'MXN';
const list_of_currency = ['GBP', 'USD', 'CNY'];
let to = list_of_currency.join();

var start_date = '2022-03-01';
var end_date = '2022-08-01'
var date_range = (start_date === "" ? 'latest?' : start_date + ".." + end_date + "?");

import { objectToString } from '@vue/shared';
import c3 from 'c3';

export default {
    //name: TheGraph,
    data() {
        return {
            data: {},
        }
    },
    beforeMount() {
        //this.getFXFromFrankfurter();
    },
    mounted() {
        this.chartFromFrankfurter();
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

        async chartFromFrankfurter() {
            const url = 'https://api.frankfurter.app/' + date_range + 'amount=1&from=' + from + "&to=" + to;
            const res = await fetch(url);
            const data = await res.json();
            var date_set = []
            var rates = [];
            var test = [];

            for (const [key, value] of Object.entries(data["rates"])) {
                let new_value = Object.assign({date : key}, value);
                test.push(new_value);
            }
            console.log(data["rates"]);
            var chart = c3.generate({
                bindto: "#lineChart",
                data: {
/*                     x: 'x',
                    //        xFormat: '%Y%m%d', // 'xFormat' can be used as custom format of 'x'
                    columns: [
                        ['x', '2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04', '2013-01-05', '2013-01-06'],
                        //            ['x', '20130101', '20130102', '20130103', '20130104', '20130105', '20130106'],
                        ['data1', 30, 200, 100, 400, 150, 250],
                        ['data2', 130, 340, 200, 500, 250, 350]
                    ] */
                    /*                     json: [
                                            { name: 'www.site1.com', upload: 200, download: 200, total: 400 },
                                            { name: 'www.site2.com', upload: 100, download: 300, total: 400 },
                                            { name: 'www.site3.com', upload: 300, download: 200, total: 500 },
                                            { name: 'www.site4.com', upload: 400, download: 100, total: 500 }
                                        ],
                                        keys: {
                                            x: 'name', // it's possible to specify 'x' when category axis
                                            value: ['upload', 'download']
                                        } */
                    json: test,
                    keys: {
                        x: 'date',
                        value: list_of_currency
                    }
                },
                axis: {
                    x: {
                        //type: 'category'
                        type: 'timeseries',
                        tick: {
                            format: '%Y-%m-%d'
                        }
                    }
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