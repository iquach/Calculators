<template>
  <div>
    <v-text-field
      class="pb-4"
      v-model.number="shares"
      label="Initial Number of Shares"
      hide-details
      outlined
      clearable
      dense
      required
    />
    <v-text-field
      class="pb-4"
      v-model.number="price"
      label="Price per share"
      prefix="$"
      hide-details
      outlined
      clearable
      dense
      required
    />
    <v-row class="pl-3 pt-3 pb-4">
      <v-text-field
        class="pb-4"
        v-model.number="annualDiv"
        label="Annual Dividend Paid"
        prefix="$"
        hide-details
        outlined
        clearable
        dense
        required
      />
      <span class="pt-2 pl-2"> Paid Monthly? </span>
      <v-checkbox class="mt-1" v-model="monthlyDiv" />
    </v-row>
    <v-text-field
      class="pb-4"
      v-model.number="annualShareGrowth"
      label="Stock Price Annual Growth Rate"
      suffix="%"
      hide-details
      outlined
      clearable
      dense
      required
    />
    <v-text-field
      class="pb-4"
      v-model.number="annualDivGrowth"
      label="Dividend Annual Growth Rate"
      suffix="%"
      hide-details
      outlined
      clearable
      dense
      required
    />
    <v-text-field
      class="pb-4"
      v-model.number="years"
      label="Number of Years"
      hide-details
      outlined
      clearable
      dense
      required
    ></v-text-field>
    <v-row>
      <v-col>
        <h4>With Dividend Reinvestment</h4>
        <div>Total Value: ${{ resultWithDiv.totalVal }}</div>
        <div>Number of Shares: {{ resultWithDiv.shares }}</div>
        <div>Dividend Paid: ${{ resultWithDiv.divPaid }}</div>
        <div>Annualized Return (%): {{ resultWithDiv.annualReturn }}</div>
        <h4>Without Divident Reinvestment</h4>
        <div>Total Value: ${{ resultWithoutDiv.totalVal }}</div>
        <div>Number of Shares: {{ resultWithoutDiv.shares }}</div>
        <div>Dividend Paid: ${{ resultWithoutDiv.divPaid }}</div>
        <div>Annualized Return (%): {{ resultWithoutDiv.annualReturn }}</div>
      </v-col>
    </v-row>
  </div>
</template>
<script>
import Vue from "vue";
export default {
  data() {
    return {
      shares: 100,
      price: 20.2,
      annualDiv: 8,
      monthlyDiv: false,
      annualShareGrowth: 5,
      annualDivGrowth: 1.1,
      years: 1,
      resultShares: 0,
    };
  },
  methods: {
    calculate() {
      this.resultShares = this.shares;
      for (let i = 0; i < this.years; i++) {
        let totalDiv = this.resultShares * this.annualDiv;
        this.resultShares += totalDiv / this.price;
      }
    },
    monthlyDivFlag(annualDiv) {
      if (this.monthlyDiv) {
        return Math.floor((annualDiv / 12) * 100) / 100;
      } else {
        return annualDiv;
      }
    },
    getXAxis() {
      let xAxis = [];
      if (this.years == 0) {
        Vue.set(xAxis, 0, 0);
      }
      for (let i = 0; i < this.years; i++) {
        Vue.set(xAxis, i, i+1);
      }
      return xAxis;
    },
    emit(val, title) {
      this.$emit(title, val);
    },
    createChartSeries(drip, seriesArr) {
      let series = null;
      if (drip) {
        series = {
          drip: true,
          name: "DRIP",
          data: seriesArr,
        };
      } else {
        series = {
          drip: false ,
          name: "Non DRIP",
          data: seriesArr,
        };
      }
      return series;
    },
  },
  computed: {
    resultWithDiv: function () {
      let shareGrowth = this.annualShareGrowth / 100;
      let divGrowth = this.annualDivGrowth / 100;
      let totalShares = this.shares;
      let divPaid = 0;
      let price = this.price;
      let annualDiv = this.annualDiv;
      let div = this.monthlyDivFlag(annualDiv);
      let months = this.monthlyDiv ? 12 : 1;
      let totalValArr = [];
      for (let i = 1; i <= this.years * months; i++) {
        let totalDiv = totalShares * div;
        divPaid += totalDiv;
        totalShares += totalDiv / price;
        if (this.monthlyDiv) {
          if (i % 12 == 0) {
            price += price * shareGrowth;
            annualDiv += annualDiv * divGrowth;
            div = this.monthlyDivFlag(annualDiv);
          }
        } else {
          price += Math.floor(price * shareGrowth * 100) / 100;
          div += Math.floor(div * divGrowth * 100) / 100;
        }

        totalShares = Math.round((totalShares + Number.EPSILON) * 100) / 100
        price = Math.round((price + Number.EPSILON) * 100) / 100
        divPaid = Math.round((divPaid + Number.EPSILON) * 100) / 100

        totalValArr[i-1] = 
            (Math.floor((totalShares * price + Number.EPSILON) * 100) / 100)
      }
      
      let val = {
        shares: Math.round((totalShares + Number.EPSILON) * 100) / 100,
        totalVal:
          Math.floor((totalShares * price + Number.EPSILON) * 100) / 100,
        divPaid: Math.round((divPaid + Number.EPSILON) * 100) / 100,
        annualReturn: 0,
      };
      this.emit(this.createChartSeries(true, totalValArr), "divSeries");

      return val;
    },
    //Coding in annual price growth and annual div growth
    resultWithoutDiv: function () {
      let divPaid = 0;
      let totalValArr = [];
      for (let i = 0; i < this.years; i++) {
        divPaid += this.shares * this.annualDiv;
        totalValArr[i] = divPaid + (this.shares * this.price);
      }

      let val = {
        shares: this.shares,
        totalVal: this.shares * this.price + divPaid,
        divPaid: divPaid,
        annualReturn: 0,
      };
      this.emit(this.getXAxis(), "xAxis");
      this.emit(this.createChartSeries(false, totalValArr), "nonDivSeries");

      return val;
    },
  },
};
</script>
<style scoped>
</style>