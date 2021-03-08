<template>
    <v-card class="pl-4 pt-4">
        <v-text-field 
            v-model.number="price"
            label="Home Price"
            outlined
        />
        <v-text-field
            v-model.number="down"
            label="Down Payment"
            outlined
            clearable
        />
        <!-- <v-text-field 
            v-model.number="downPercentage"
            label=""
        /> -->
        <v-text-field
            v-model.number="length"
            label="Lenght of Loan"
            outlined
            clearable
        />
        <v-text-field 
            v-model.number="interest"
            label="Interest Rate"
            outlined
            clearable
        />
        <v-btn color="primary" @click="calculate">Submit</v-btn>
        <v-row>
            <v-col>
                {{result}}
            </v-col>
        </v-row>
    </v-card>
</template>
<script>
export default {
    data() {
        return {
             price: null,
             down: null,
             downPercentage: null,
             length: null,
             interest: null,
             result: 0
        }
    },
    methods:{
        calculate() {
            let monthlyInterest = (this.interest / 100) / 12; 
            let totalMonths = this.length * 12;
            let parentPow = Math.pow((1 + monthlyInterest), -totalMonths);
            let top = this.price * monthlyInterest;
            let botton = 1 - parentPow;
            this.result = top / botton;
        }
    }
}
</script>
<style scoped>

</style>