<template>
    <v-card>
        <h3>Enter following information for dividend calculator</h3>
        <v-text-field
            v-model.number="shares"
            label="Shares"
            outlined
            clearable
        ></v-text-field>
        <v-text-field
            v-model.number="price"
            label="Price per share"
            outlined
            clearable
        ></v-text-field>
        <v-text-field
            v-model.number="annualDiv"
            label="Annual Dividend"
            outlined
            clearable
        ></v-text-field>
        <v-text-field
            v-model.number="years"
            label="Number of Years"
            outlined
            clearable
        ></v-text-field>
        <v-row>
        <v-btn @click="calculate()"
        >
            Submit
        </v-btn>
        </v-row>
        <v-row>
            <v-col>
                Shares: {{resultShares}}
            </v-col>
        </v-row>
    </v-card>
</template>
<script>
export default {
    data() {
        return {
            shares: null,
            price: null,
            annualDiv: null,
            years: null,
            resultShares: 0
        }
    },
    methods: {
        calculate() {
            this.resultShares = this.shares;
            for(let i = 0; i < this.years; i++) {
                let totalDiv = this.resultShares * this.annualDiv;
                this.resultShares += totalDiv / this.price;
            }
        }
    }
}
</script>
<style scoped>

</style>