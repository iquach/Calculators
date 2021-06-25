<template>
  <div>
    <h1 class="pl-4">Dividend Reinvestment Calculator (DRIP)</h1>
    <v-row class="pl-4 pt-2" no-gutters>
      <v-col cols="4">
        <forms
          v-on:nonDivSeries="getSeries"
          v-on:divSeries="getSeries"
          v-on:xAxis="getXAxis"
        />
      </v-col>
      <v-col cols="8">
        <chart :series="series" :xAxis="xAxis" />
      </v-col>
    </v-row>
  </div>
</template>
<script>
import Vue from 'vue';
import forms from "@/components/Dividend/Form";
import chart from "@/components/Dividend/Chart";
export default {
  components: {
    forms,
    chart,
  },
  data() {
    return {
      series: [
        {
          name: "DRIP",
          data: [2961]
        },
        {
          name: "Non DRIP",
          data: [2820]
        }
      ],
      xAxis: [1],
    };
  },
  methods: {
    getSeries(val) {
      if(val.drip) {
        Vue.set(this.series[0], "data", val.data);
      } else if(!val.drip) {
        Vue.set(this.series[1], "data", val.data);
      }
    },
    getXAxis(val) {
      this.xAxis = val;
    },
  },
};
</script>
<style scoped>
</style>