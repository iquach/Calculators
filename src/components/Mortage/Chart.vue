<template>
    <apexchart width=620 :options="chartOptions" :series="series"></apexchart>
</template>
<script>
export default {
    components:{
    },
    props: {
        series: {
            type: Array,
            required: true,
            default: function () {
                return [1756.23, 66, 329, 100]
            }
        }
    },
    data() {
        return {
            chartOptions: {
                labels: ['Principal & interest', "Homeowner's insurance", 'Property tax', 'HOA fees'],
                chart: {
                    type: 'donut',
                    animations: {
                        speed: 480
                    }
                },
                tooltip: {
                    enabled: false
                },
                plotOptions: {
                    pie: {
                        donut: {
                            labels: {
                                show: true,
                                name: {
                                    show: true,
                                    offsetY: -10,
                                    formatter: function (val) {
                                        return val+":"
                                    }
                                },
                                value: {
                                    show: true,
                                    formatter: function (val) {
                                        return "$"+val
                                    }
                                },
                                total: {
                                    show: true,
                                    showAlways: false,
                                    label: 'Monthly Payment',
                                    fontSize: '20px',
                                    fontFamily: 'Helvetica, Arial, sans-serif',
                                    fontWeight: 550,
                                    color: '#373d3f',
                                    formatter: function (w) {
                                        return "$"+w.globals.seriesTotals.reduce((a, b) => {
                                        return Math.round((a + b) * 100.0) / 100.0;
                                        }, 0)
                                    }
                                }
                            }
                        }
                    }
                },
                responsive: [{
                    options: {
                        legend: {
                        position: 'bottom'
                        }
                    }
                }]
            }
        }
    }
}
</script>