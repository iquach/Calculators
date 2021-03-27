<template>
  <div>
    <apexchart
      ref="realtimeChart"
      type="bar"
      height="350"
      :options="chartOptions"
      :series="series"
    ></apexchart>
  </div>
</template>
<script>
import Vue from 'vue';
export default {
  props: {
    series: {
      type: Array,
      required: true,
    },
    xAxis: {
      type: Array,
      required: true,
    }
  },
  data() {
    return {
      chartOptions: {
        chart: {
          type: "bar",
          height: 350,
        },
        plotOptions: {
          bar: {
            horizontal: false,
            columnWidth: "100%",
            endingShape: "rounded",
          },
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          show: true,
          width: 2,
          colors: ["transparent"],
        },
        xaxis: {
          title: {
            text: "Years",
          },
          categories: ["1"],
        },
        yaxis: {
          title: {
            text: "$ (thousands)",
          },
        },
        fill: {
          opacity: 1,
        },
        tooltip: {
          y: {
            formatter: function (val) {
              return "$ " + val + " thousand";
            },
          },
        },
      },
    };
  },
  watch: {
    xAxis: function() {
      Vue.set(this.chartOptions.xaxis, 'categories', this.xAxis);
      this.$refs.realtimeChart.updateOptions(this.chartOptions,true, true, true);
    }
  }
};
</script>
<style scoped>
</style>