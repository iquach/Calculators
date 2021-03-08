<template>
    <v-row>
        <v-col>
            <v-card  class="pa-8">
                <h3>Enter following information for dividend calculator</h3>
                <v-text-field
                    v-model.number="shares"
                    label="Shares"
                    outlined
                    clearable
                    required
                ></v-text-field>
                <v-text-field
                    v-model.number="price"
                    label="Price per share"
                    outlined
                    clearable
                    required
                ></v-text-field>
                <v-text-field
                    v-model.number="annualDiv"
                    label="Annual Dividend"
                    outlined
                    clearable
                    required
                ></v-text-field>
                <v-text-field
                    v-model.number="years"
                    label="Number of Years"
                    outlined
                    clearable
                    required
                ></v-text-field>
                <v-btn 
                    color="#4295f5" 
                    @click="calculate()"
                >
                    Submit
                </v-btn>
                <v-row>
                    <v-col>
                        Shares: {{resultShares}}
                    </v-col>
                </v-row>
            </v-card>
        </v-col>
    </v-row>
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