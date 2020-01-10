<template> 
    <div>
       <canvas class='chart' :id="chart_id"></canvas>
       <!-- <canvas class='chart' id="chart2"></canvas>
       <canvas class='chartid="chart3"></canvas> -->
    </div> 
    
</template>

<script>

export default {
    name: "Gauge", 
    props: ["chart_id", "metric"],
    data() {
        return {
            charts: [0,0,0,0],
            raw_data: [[35, 21, 27, 55, 43, 65, 28, 34, 57, 42, 50, 54, 49, 77, 50, 74, 45, 88, 75], 
            [38, 54, 64, 58, 57, 52, 35, 38, 67, 55, 9, 42, 52, 30, 55, 10, 30, 80, 45], 
            [24, 30, 35, 50, 61, 35, 31, 60, 63, 34, 67, 51, 73, 35, 24, 32, 50, 36, 63]],
            xaxis: [ 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18],
            metric_labels: ["Focus", "Load", "Fatigue"],
            borderColors: ['rgba(75, 192, 192, 1)',  'rgba(255, 159, 64, 1)','rgba(153, 102, 255, 1)'],
            backgroundColors: ['rgba(75, 192, 192, 0.2)', 'rgba(255, 159, 64, .2)', 'rgba(153, 102, 255, 0.2)'],
            // num_hours: 5,
            // num_day: 12
        }
    },
    mounted() {
        this.renderPartialDonut(this.chart_id,this.metric);
        // this.renderPartialDonut("chart2","Load");
        // this.renderPartialDonut("chart3","Fatigue");
        // render_combined_donut("chart2", [0,1,2]);

        

    },
    methods: {
        renderPartialDonut: function(ctx_id, metric) {
            Chart.pluginService.register({
                afterUpdate: function (chart) {
                    if (chart.config.options.elements.center) {
                        var helpers = Chart.helpers;
                        var centerConfig = chart.config.options.elements.center;
                        var globalConfig = Chart.defaults.global;
                        var ctx = chart.chart.ctx;

                        var fontStyle = helpers.getValueOrDefault(centerConfig.fontStyle, globalConfig.defaultFontStyle);
                        var fontFamily = helpers.getValueOrDefault(centerConfig.fontFamily, globalConfig.defaultFontFamily);

                        if (centerConfig.fontSize)
                            var fontSize = centerConfig.fontSize;
                            // figure out the best font size, if one is not specified
                        else {
                            ctx.save();
                            var fontSize = helpers.getValueOrDefault(centerConfig.minFontSize, 1);
                            var maxFontSize = helpers.getValueOrDefault(centerConfig.maxFontSize, 256);
                            var maxText = helpers.getValueOrDefault(centerConfig.maxText, centerConfig.text);

                            do {
                                ctx.font = helpers.fontString(fontSize, fontStyle, fontFamily);
                                var textWidth = ctx.measureText(maxText).width;

                                // check if it fits, is within configured limits and that we are not simply toggling back and forth
                                if (textWidth < chart.innerRadius * 2 && fontSize < maxFontSize)
                                    fontSize += 1;
                                else {
                                    // reverse last step
                                    fontSize -= 1;
                                    break;
                                }
                            } while (true)
                            ctx.restore();
                        }

                        // save properties
                        chart.center = {
                            font: helpers.fontString(fontSize, fontStyle, fontFamily),
                            fillStyle: helpers.getValueOrDefault(centerConfig.fontColor, globalConfig.defaultFontColor)
                        };
                    }
                },
                afterDraw: function (chart) {
                    if (chart.center) {
                        var centerConfig = chart.config.options.elements.center;
                        var ctx = chart.chart.ctx;

                        ctx.save();
                        ctx.font = chart.center.font;
                        ctx.fillStyle = chart.center.fillStyle;
                        ctx.textAlign = 'center';
                        ctx.textBaseline = 'middle';
                        var centerX = (chart.chartArea.left + chart.chartArea.right) / 2;
                        var centerY = (chart.chartArea.top + chart.chartArea.bottom) / 2;
                        ctx.fillText(centerConfig.text, centerX, centerY);
                        ctx.restore();
                    }
                },
            });
            // if (chartnum == 3) {
            //     ctx = document.getElementById("chart3");
            // } else if (chartnum == 4) {
            //     ctx = document.getElementById("chart4");
            // } else if (chartnum == 5) {
            //     ctx = document.getElementById("chart5");
            // } 
            let ctx = document.getElementById(ctx_id);

            var ind = 0;
            if (metric == "Focus") {
                ind = 0;
            } else if (metric == "Load") {
                ind = 1;
            } else if (metric == "Fatigue") {
                ind =2;
            }
            

            let donut_data = [[this.raw_data[0].slice(-1)[0], 100-this.raw_data[0].slice(-1)[0]], [this.raw_data[1].slice(-1)[0], 100-this.raw_data[1].slice(-1)[0]], [this.raw_data[2].slice(-1)[0], 100-this.raw_data[2].slice(-1)[0]]];
            // console.log(donut_data);
            var scale = .6;
            // var converted_data = donut_data.map((lst)=>{return lst.map((val)=>{return val*scale})});
            // console.log(converted_data);
            // converted_data.forEach((lst)=>{lst.push(100-scale*100)});
            // console.log(donut_data[ind]);
            var donut_data_obj = {
                datasets: [
                    {
                        data: donut_data[ind],
                        label: this.metric_labels[ind],
                        backgroundColor: [
                            this.backgroundColors[ind],
                            "rgba(0, 0, 0, 0)",
                            "rgba(0, 0, 0, 0)"
                        ],
                        borderColor: [
                            this.borderColors[ind],
                            this.borderColors[ind],
                            "rgba(0, 0, 0, 0)",
                        ],
                        hoverBackgroundColor: [
                            this.borderColors[ind],
                            "rgba(0, 0, 0, 0)",
                            "rgba(0, 0, 0, 0)"
                        ],
                        // hoverBorderColor: [,"rgba(0, 0, 0, 0)"]
                    }]
            };
            var myDonutChart = new Chart(ctx, {
                type: 'doughnut',
                data: donut_data_obj,
                options: {
                    // plugins: {
                    //     // Change options for ALL labels of THIS CHART
                    //     datalabels: {
                    //         formatter: function(value, context) {
                    //             console.log(context);
                    //             console.log(value);
                    //             if (context.dataIndex == 0) {
                    //                 return value;
                    //             }
                    //             return "";
                    //         }
                    //     },
                    // },
                    legend: {
                        display: false
                    },
                    tooltips: {
                        callbacks: {
                            label: function(item, data) {
                                // console.log("Item thing: ", item, item.index);
                                // console.log("data here!!! ", data);
                                // console.log(data.datasets);
                                if (item.index == 0){
                                    return data.datasets[0].label+ ": "+ data.datasets[item.datasetIndex].data[item.index];
                                }
                                return "";
                                
                            }
                        }
                    },
                    cutoutPercentage: 80,
                    rotation: -.5*Math.PI-.6*Math.PI,
                    circumference: 2*Math.PI*.6,
                    animation: {
                        animationRotate: true,
                        duration: 2000
                    },
                    elements: {
                        center: {
                            // the longest text that could appear in the center
                            maxText: '100%',
                            text: this.metric_labels[ind] +": " +donut_data[ind][0],
                            fontColor: this.borderColors[ind],
                            fontFamily: "'Helvetica Neue', 'Helvetica', 'Arial', sans-serif",
                            fontStyle: 'normal',
                            // fontSize: 12,
                            // if a fontSize is NOT specified, we will scale (within the below limits) maxText to take up the maximum space in the center
                            // if these are not specified either, we default to 1 and 256
                            minFontSize: 1,
                            maxFontSize: 34,
                        },
                    }
                }
            });
            this.charts[ind] = myDonutChart;
        }
    }
}
</script>
<style scoped>


</style>