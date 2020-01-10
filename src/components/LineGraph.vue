<template> 
    <div>
       <canvas class='chart' :id="chart_id"></canvas>
       <!-- <canvas class='chart' id="chart2"></canvas>
       <canvas class='chartid="chart3"></canvas> -->
    </div> 
    
</template>

<script>

export default {
    name: "LineGraph", 
    props: ["chart_id"],
    data() {
        return {
            // charts: [0,0,0,0],
            raw_data: [[35, 21, 27, 55, 43, 65, 28, 34, 57, 42, 50, 54, 49, 77, 50, 74, 45, 88, 75], 
            [38, 54, 64, 58, 57, 52, 35, 38, 67, 55, 9, 42, 52, 30, 55, 10, 30, 80, 45], 
            [24, 30, 35, 50, 61, 35, 31, 60, 63, 34, 67, 51, 73, 35, 24, 32, 50, 36, 63]],
            xaxis: [ 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18],
            metric_labels: ["Focus", "Load", "Fatigue"],
            borderColors: ['rgba(75, 192, 192, 1)',  'rgba(255, 159, 64, 1)','rgba(153, 102, 255, 1)'],
            backgroundColors: ['rgba(75, 192, 192, 0.2)', 'rgba(255, 159, 64, .2)', 'rgba(153, 102, 255, 0.2)'],
            num_hours: 5,
            num_day: 12,
            lineChart: ""
        }
    },
    mounted() {
        this.renderLine(this.chart_id);
        // this.lineWeek();
    },
    methods: {
        renderLine: function(canvas_id) {   
            var ctx = document.getElementById(canvas_id).getContext('2d');
            this.lineChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: this.xaxis.slice(-1*this.num_hours),
                    datasets: [
                        {
                        label: this.metric_labels[0]+"hour",
                        data: this.raw_data[0].slice(-1*this.num_hours),
                        hidden: false,
                        borderColor: this.borderColors[0],
                        backgroundColor: 'rgba(0, 0, 0, 0)',
                        },
                        {
                        label: this.metric_labels[1]+"hour",
                        data: this.raw_data[1].slice(-1*this.num_hours),
                        hidden: false,
                        borderColor: this.borderColors[1],
                        backgroundColor: 'rgba(0, 0, 0, 0)',
                        },
                        {
                        label: this.metric_labels[2]+"hour",
                        data: this.raw_data[2].slice(-1*this.num_hours),
                        hidden: false,
                        borderColor: this.borderColors[2],
                        backgroundColor: 'rgba(0, 0, 0, 0)',
                        },
                        {
                        label: this.metric_labels[0]+"days",
                        data: this.raw_data[0].slice(-1*this.num_day),
                        hidden: true,
                        borderColor: this.borderColors[0],
                        backgroundColor: 'rgba(0, 0, 0, 0)',
                        },
                        {
                        label: this.metric_labels[1]+"days",
                        data: this.raw_data[1].slice(-1*this.num_day),
                        hidden: true,
                        borderColor: this.borderColors[1],
                        backgroundColor: 'rgba(0, 0, 0, 0)',
                        },
                        {
                        label: this.metric_labels[2]+"days",
                        data: this.raw_data[2].slice(-1*this.num_day),
                        hidden: true,
                        borderColor: this.borderColors[2],
                        backgroundColor: 'rgba(0, 0, 0, 0)',
                        },
                        {
                        label: this.metric_labels[0]+"week",
                        data: this.raw_data[0].slice(7),
                        hidden: true,
                        borderColor: this.borderColors[0],
                        backgroundColor: 'rgba(0, 0, 0, 0)',
                        },
                        {
                        label: this.metric_labels[1]+"week",
                        data: this.raw_data[1].slice(7),
                        hidden: true,
                        borderColor: this.borderColors[1],
                        backgroundColor: 'rgba(0, 0, 0, 0)',
                        },
                        {
                        label: this.metric_labels[2]+"week",
                        data: this.raw_data[2].slice(7),
                        hidden: true,
                        borderColor: this.borderColors[2],
                        backgroundColor: 'rgba(0, 0, 0, 0)',
                        }
                    ]
                },
                options: {
                    plugins: {
                        // Change options for ALL labels of THIS CHART
                        datalabels: {
                            formatter: function(value, context) {
                                return "";
                            }
                        },
                    },
                    title: {
                        text: "Hourly Cognitive Report",
                        display: false,
                        fontSize: 24,
                        // lineHeight
                    },
                    legend: {
                        display: true,
                        position: 'bottom',
                        labels: {
                            filter: function(item, data) {
                                // console.log("item: ", item);
                                // console.log("data: ", data);
                                if (item.text.slice(-4) == "hour") {
                                    // item.fillStyle = backgroundColors[metric_ind[item.index]];
                                    // item.strokeStyle = borderColors[metric_ind[item.index]];
                                    item.text = item.text.slice(0,-4);
                                    item.hidden = false;
                                    // console.log("New: ", item)
                                    return true;
                                    // return data.labels[item.index];
                                }
                                item.hidden = true;
                                return false;
                                
                            }
                        }
                    },           
                    scales: {
                        yAxes: [{
                            ticks: {
                                beginAtZero: true,
                                min: 0,
                                max: 100,
                                // callback: function(value, index, values) {
                                //     return float2dollar(value);
                                // }
                            }
                        }],
                        xAxes: [{
                            ticks: {
                                callback: function(value, index, values) {
                                    if (typeof value == 'number') {
                                        if (Math.floor(value) == value) {
                                            return value + ":00";
                                        }
                                    } else {
                                        return value;
                                    }
                                    
                                }
                            }
                        }]                 
                    }
                },
            });
        },

        lineHours: function() {
            this.lineChart.data.labels= xaxis.slice(-1*this.num_hours);
            this.lineChart.data.datasets.forEach((dataset) => {
                if (dataset.label.slice(-4) == "hour"){
                    dataset.hidden = false;
                } else {
                    dataset.hidden = true;
                }
            });
            this.lineChart.update();
        },
        LineDay: function() {
            this.lineChart.data.labels= xaxis.slice(-1*this.num_day);
            this.lineChart.data.datasets.forEach((dataset) => {
                if (dataset.label.slice(-4) == "days"){
                    dataset.hidden = false;
                } else {
                    dataset.hidden = true;
                }
            });
            this.lineChart.update();
        },
        lineWeek: function () {
            this.lineChart.data.labels= ["Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday"];
            this.lineChart.data.datasets.forEach((dataset) => {
                if (dataset.label.slice(-4) == "week"){
                    dataset.hidden = false;
                } else {
                    dataset.hidden = true;
                }
            });
            this.lineChart.update();
        }
    }
}
</script>
<style scoped>


</style>