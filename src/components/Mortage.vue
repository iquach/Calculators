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
                    v-model.number="length"
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
             downPercentage: 20,
             length: 30,
             interest: 3.32
            //  result: 0
        }
    },
    methods:{
        // calculate() {
        //     let principle = this.price - this.down;
        //     let monthlyInterest = (this.interest / 100) / 12; 
        //     let totalMonths = this.length * 12;
        //     let parentPow = Math.pow((1 + monthlyInterest), -totalMonths);
        //     let top = principle * monthlyInterest;
        //     let botton = 1 - parentPow;
        //     this.result = Math.ceil((top / botton) * 100) / 100;
        // }
    },
    watch: {
        downPercentage: function(val) {
            this.down = (val / 100) * this.price;
        }
        // price: function(val) {
        //     this.down = (this.downPercentage/ 100) * val;
        // }
    },
    computed: {
        result: function() {
            let principle = this.price - this.down;
            let monthlyInterest = (this.interest / 100) / 12; 
            let totalMonths = this.length * 12;
            let parentPow = Math.pow((1 + monthlyInterest), -totalMonths);
            let top = principle * monthlyInterest;
            let botton = 1 - parentPow;
            return Math.ceil((top / botton) * 100) / 100;
        }
    }
}
</script>
<style scoped>

</style>