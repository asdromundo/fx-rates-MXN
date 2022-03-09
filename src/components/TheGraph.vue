<template>
    <div id="home">
        <div class="chart1">
            <div id="lineChart"></div>
        </div>
    </div>
</template>
<script>

var from = 'MXN';
const list_of_currency = ['GBP', 'USD', 'CNY'];
let to = list_of_currency.join();

var start_date = '2020-03-01';
var end_date = '2021-01-01'
var date_range = (start_date === "" ? 'latest?' : start_date + ".." + end_date + "?");

import c3 from 'c3';

export default {
    data() {
        return {
            data: {},
        }
    },
    mounted() {
        this.chartFromFrankfurter();
    },
    methods: {

        async chartFromFrankfurter() {
            const url = 'https://api.frankfurter.app/' + date_range + 'amount=1&from=' + from + "&to=" + to;
            const res = await fetch(url);
            const data = await res.json();
            var dataset = [];

            for (const [key, value] of Object.entries(data["rates"])) {
                let new_value = Object.assign({ date: key }, value);
                dataset.push(new_value);
            }
            console.log(data["rates"]);
            var chart = c3.generate({
                bindto: "#lineChart",
                data: {
                    json: dataset,
                    keys: {
                        x: 'date',
                        value: list_of_currency
                    }
                },
                axis: {
                    x: {
                        label: {
                            text: 'Time interval',
                            position: 'center'
                        },
                        type: 'timeseries',
                        tick: {
                            multiline: true,
                            culling: {max: 10},
                            fit: false,
                            format: '%d-%m-%y'
                        }
                    },
                    y: {
                        label: {
                            text: 'Fx rate',
                            position: 'center'
                        }
                    }
                },
                point: {
                    show: false
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
#lineChart .c3-line {
    stroke-width: 3px;
}
#lineChart .c3-axis path,
line {
    stroke-width: 2px;
    stroke:black;
}
#lineChart .c3-axis text {
    font-family: sans-serif;
    font-size: 14px;
}
</style>