<template>
    <v-row>
        <v-col cols="4"></v-col>
        <v-col cols="3">
            <v-card class="pl-4 pt-4">
                <v-text-field 
                    v-model.number="price"
                    label="Home Price"
                    prefix="$"
                    outlined
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
                <v-text-field
                    v-model.number="months"
                    label="Lenght of Loan"
                    outlined
                />
                <v-text-field 
                    v-model.number="interest"
                    label="Interest Rate"
                    suffix="%"
                    outlined
                />
                <!-- <v-btn color="primary" @click="calculate">Submit</v-btn> -->
                <v-row class="pt-2">
                    <v-col>
                        Monthly Payment: ${{result}}
                    </v-col>
                </v-row>
            </v-card>
        </v-col>
        <!-- <v-col>
        </v-col> -->
    </v-row>
</template>
<script>
export default {
    data() {
        return {
             price: 500000,
             down: 100000,
             displayDown:100000,
             downPercentage: 20,
             displayDownPercentage: 20,
             months: 30,
             interest: 3.32,
             changeFlag: false
        }
    },
    methods:{
        // calculate() {
        //     let principle = this.price - this.down;
        //     let monthlyInterest = (this.interest / 100) / 12; 
        //     let totalMonths = this.months * 12;
        //     let parentPow = Math.pow((1 + monthlyInterest), -totalMonths);
        //     let top = principle * monthlyInterest;
        //     let botton = 1 - parentPow;
        //     this.result = Math.ceil((top / botton) * 100) / 100;
        // }
    },
    watch: {
        downPercentage: function(val) {
            if(!this.changeFlag) {
                // if(val == '.') {
                //     this.downPercentage = 0.0;
                //     val = 0.0;
                //     console.log("here")
                //     console.log(val);
                //     console.log(this.downPercentage);
                // }
                let temp = (val / 100) * this.price;
                // this.down =  Math.floor(temp);
                this.down = temp;
                this.test = 2020202020
                this.changeFlag = !this.changeFlag;
            } else {
                this.changeFlag = !this.changeFlag;
            }
        },
        down: function(val) {
            if(!this.changeFlag) {
                let temp = (val / this.price) * 100;
                this.downPercentage = Math.round(temp * 100.0) / 100.0;
                this.changeFlag = false;
                // this.downPercentage = temp;
                this.changeFlag = !this.changeFlag;
            } else {
                this.changeFlag = !this.changeFlag;
            }
        }
    },
    computed: {
        result: function() {
            let p = this.price - this.down;
            let r = (this.interest / 100) / 12;
            let n = this.months * 12;
            let pr = p * r;
            let parentPow = Math.pow((1 + r), n);
            let top = pr * parentPow;
            let bottom = parentPow - 1;
            let val = Math.ceil((top / bottom) * 100) / 100;
            return val;
        },
    }
}
</script>
<style scoped>
</style>