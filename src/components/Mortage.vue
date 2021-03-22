<template>
<div>
    <h1 class="pl-4">Mortage Calculator</h1>
    <v-row class="pl-4 pt-4">
        <v-col cols="4" >
            <v-text-field 
                v-model.number="price"
                label="Home Price"
                prefix="$"
                outlined
                :rules="[v => !!v || 'Home price is required']"
                required
            />
            <v-row>
                <v-col cols="8">
                    <v-text-field
                        v-model.number="down"
                        value="test"
                        label="Down Payment"
                        prefix="$"
                        outlined
                    />
                </v-col>
                <v-col>
                    <v-text-field 
                        v-model.number="downPercentage"
                        label="Percent"
                        suffix="%"
                        outlined
                    />
                </v-col>
            </v-row>
            <v-select
                v-model="loanLength"
                :items="loanLengths"
                label="Outlined style"
                outlined
            />
            <v-text-field 
                v-model.number="interest"
                label="Interest Rate"
                suffix="%"
                outlined
                :rules="[v => !!v || 'Interest cannot be empty.']"
            />
            <v-row>
                <v-col>
                    Monthly Payment: ${{result}}
                </v-col>
            </v-row>
            <v-text-field 
                v-model.number="insurance"
                label="Homeowner's Insurance"
                outlined
                prefix="$"
                class="pt-4"
            />
            <v-text-field 
                v-model.number="tax"
                label="Property Tax"
                outlined
                prefix="$"
            />
            <v-text-field 
                v-model.number="hoa"
                label="HOA Fees"
                outlined
                prefix="$"
            />
        </v-col>
        <v-col cols="8">
            <apexchart ref="realtimeChart" width=620 :options="chartOptions" :series="series"></apexchart>
        </v-col>
    </v-row>
</div>
</template>
<script>
import apexchart from 'vue-apexcharts'
export default {
    components: {
        apexchart
    },
    data() {
        return {
            price: 500000,
            down: 100000,
            displayDown:100000,
            downPercentage: 20,
            displayDownPercentage: 20,
            loanLength: 30,
            loanLengths: [40, 30, 25, 20, 15, 10],
            interest: 3.32,
            insurance: 66,
            tax: 329,
            hoa: 100,
            changeFlag: false,
            series:  [1756.23, 66, 329, 100],
            
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
    },
    methods:{
        updateSeriesLine() {
            this.$refs.realtimeChart.updateSeries(this.series, false, true);
        }
    },
    watch: {
        downPercentage: function(val) {
            if(val == "") {
                this.downPercentage = 0;
            }
            if(!this.changeFlag) {
                if(val == '.') {
                    this.downPercentage = 0.0;
                    val = 0.0;
                }
                let temp = (val / 100) * this.price;
                this.down = temp;
                this.test = 2020202020
                this.changeFlag = !this.changeFlag;
            } else {
                this.changeFlag = !this.changeFlag;
            }
        },
        down: function(val) {
            if(val == "") {
                this.down = 0;
            }
            if(!this.changeFlag) {
                let temp = (val / this.price) * 100;
                this.downPercentage = Math.round(temp * 100.0) / 100.0;
                this.changeFlag = false;
                this.changeFlag = !this.changeFlag;
            } else {
                this.changeFlag = !this.changeFlag;
            }
        },
        result: function(val) {
            this.series[0] = val;
            this.updateSeriesLine();
        },
        insurance: function(val) {
            if(val == "") {
                val = 0;
            }
            this.series[1] = val;
            this.updateSeriesLine();
        },
        tax: function(val) {
            if(val == "") {
                val = 0;
            }
            this.series[2] = val;
            this.updateSeriesLine();
        },
        hoa: function(val) {
            if(val == "") {
                val = 0;
            }
            this.series[3] = val;
            this.updateSeriesLine();
        },
        price: function(val) {
            if(val == ""){
                this.price = 0;
            }
        },
        interest: function(val) {
            if(val == "") {
                this.interest = 0;
            }
        }
    },
    computed: {
        result: function() {
            let p = this.price - this.down;
            let r = (this.interest / 100) / 12;
            let n = this.loanLength * 12;
            let pr = p * r;
            let parentPow = Math.pow((1 + r), n);
            let top = pr * parentPow;
            let bottom = parentPow - 1;
            let val = Math.ceil((top / bottom) * 100) / 100;
            return val;
        }
    }
}
</script>
<style scoped>
</style>