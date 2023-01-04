<template>
    <div>
        <v-alert type="info" v-if="montos.length == 0">
            Aún no hay montos para mostrar, use el formulario para ingresar su primer monto
        </v-alert>

        <div v-show="montos.length > 0" id="chart" ref="chart"></div>
    </div>
</template>

<style scoped>
#chart {
    width: 100%;
    height: 500px;
}
</style>

<script>
import axios from 'axios'
import * as am5 from '@amcharts/amcharts5'
import * as am5xy from '@amcharts/amcharts5/xy';
import am5themes_Animated from '@amcharts/amcharts5/themes/Animated';
import am5themes_Dark from "@amcharts/amcharts5/themes/Dark";
import am5locales_en_ES from "@amcharts/amcharts5/locales/es_ES";

export default {
    name: 'MontosGrafico',
    data() {
        return {
            montos: [],
            root: null,
        }
    },
    methods: {
        renderGrafico() {
            axios.get('https://position-backend-production.up.railway.app/api/mis-montos', {
            headers: {
                Authorization: 'Bearer ' + localStorage.getItem('token')
            },
            }).then(response => {

                if (response.status == 200 && response.data.montos.length != 0) {
                    this.montos = response.data.montos

                    let root = am5.Root.new(this.$refs.chart)

                    root.locale = am5locales_en_ES;

                    root.setThemes([
                        am5themes_Animated.new(root),
                        am5themes_Dark.new(root)
                    ]);

                    root.dateFormatter.setAll({
                        dateFormat: 'dd-MM-yyyy',
                        dateFields: ['valueX']
                    });

                    var chart = root.container.children.push(am5xy.XYChart.new(root, {
                        focusable: true,
                        panX: true,
                        panY: true,
                        wheelX: "panX",
                        wheelY: "zoomX",
                        pinchZoomX:true
                    }))

                    // var easing = am5.ease.linear

                    var xAxis = chart.xAxes.push(am5xy.DateAxis.new(root, {
                        maxDeviation: 0.1,
                        groupData: false,
                        baseInterval: {
                            timeUnit: "day",
                            count: 1
                        },
                        renderer: am5xy.AxisRendererX.new(root, {

                        }),
                        tooltip: am5.Tooltip.new(root, {})
                    }));

                    var yAxis = chart.yAxes.push(am5xy.ValueAxis.new(root, {
                        maxDeviation: 0.2,
                        renderer: am5xy.AxisRendererY.new(root, {})
                    }));

                    var series = chart.series.push(am5xy.LineSeries.new(root, {
                        minBulletDistance: 10,
                        connect: false,
                        xAxis: xAxis,
                        yAxis: yAxis,
                        valueYField: "monto",
                        valueXField: "fecha",
                        tooltip: am5.Tooltip.new(root, {
                            pointerOrientation: "horizontal",
                            labelText: "{valueY}"
                        })
                    }));

                    series.fills.template.setAll({
                        fillOpacity: 0.2,
                        visible: true
                    });

                    series.strokes.template.setAll({
                        strokeWidth: 2
                    });

                    series.data.processor = am5.DataProcessor.new(root, {
                        dateFormat: "dd-MM-yyyy",
                        dateFields: ["fecha"]
                    });

                    series.data.setAll(this.montos);

                    series.bullets.push(function() {
                        var circle = am5.Circle.new(root, {
                            radius: 4,
                            fill: root.interfaceColors.get("background"),
                            stroke: series.get("fill"),
                            strokeWidth: 2
                        })

                        return am5.Bullet.new(root, {
                            sprite: circle
                        })
                    });

                    var cursor = chart.set("cursor", am5xy.XYCursor.new(root, {
                        xAxis: xAxis,
                        behavior: "none"
                    }));

                    cursor.lineY.set("visible", false);
                    
                    chart.set("scrollbarX", am5.Scrollbar.new(root, {
                        orientation: "horizontal"
                    }));

                    chart.appear(1000, 100);

                    this.root = root
                }
            })
        },
        reRender() {
            this.$forceUpdate()
        }
    },
    mounted() {
        this.renderGrafico()

        this.$root.$on('re-render-grafico', () => {
            this.renderGrafico()
            this.root.dispose()
        })
        
    }
}
</script>