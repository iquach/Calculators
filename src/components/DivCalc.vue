<template>
    <div>
        <h1 class="pl-4">Dividend Reinvestment Calculator (DRIP)</h1>
        <v-row class="pl-4 pt-2" no-gutters>
            <v-col cols="4">
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
                        suffix="%"
                        hide-details
                        outlined
                        clearable
                        dense
                        required
                    />
                    <span class="pt-2 pl-2">
                        Paid Monthly?
                    </span>
                    <v-checkbox class="mt-1" v-model="monthlyDiv"/>
                </v-row>
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
                        <div>
                            Total Value: ${{resultWithDiv.totalVal}}
                        </div>
                        <div>
                            Number of Shares: {{resultWithDiv.shares}} 
                        </div>
                        <div>
                            Dividend Paid: ${{resultWithDiv.divPaid}}
                        </div> 
                        <div>
                            Annualzied Return (%): {{resultWithDiv.annualReturn}}
                        </div>
                        <h4>Without Divident Reinvestment </h4>
                        <div>
                            Total Value: {{resultWithoutDiv.totalVal}}
                        </div>
                        <div>
                            Number of Shares: {{resultWithoutDiv.shares}}
                        </div>
                        <div>
                            Dividends Paid: {{resultWithoutDiv.divPaid}}
                        </div>
                        <div>
                            Annualized Return (%): {{resultWithoutDiv.annualReturn}}
                        </div>
                    </v-col>
                </v-row>
            </v-col>
        </v-row>
    </div>
</template>
<script>
export default {
    data() {
        return {
            shares: 100,
            price: 20.20,
            annualDiv: 8,
            monthlyDiv: false,
            years: 1,
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
        },
        monthlyDivFlag() {
            if (this.monthlyDiv) {
                return Math.floor((this.annualDiv / 12)* 100) / 100;
            } else {
                return this.annualDiv;
            }
        }
    },
    computed: {
        resultWithDiv: function() {
            let totalShares = this.shares;
            let divPaid = 0;
            let months = 0;
            let price = this.price
            let div = 0;
            div = this.monthlyDivFlag();
            months = this.monthlyDiv ? 12 : 1;
            for(let i = 0; i < (this.years*months); i++) {
                let totalDiv = totalShares * div;
                divPaid += totalDiv;
                totalShares += totalDiv / price;
            }  
            let val = {
                shares: Math.round((totalShares + Number.EPSILON) * 100) / 100,
                totalVal: Math.round(((totalShares * price) + Number.EPSILON) * 100) / 100,
                divPaid: Math.round((divPaid + Number.EPSILON) * 100) / 100,
                annualReturn: 0
            }

            return val;
        },
        resultWithoutDiv: function() {
            let divPaid = 0;
            for (let i = 0; i < this.years; i++) {
                divPaid += this.shares * this.annualDiv;
            }
            
            let val ={
                shares: this.shares,
                totalVal: (this.shares * this.price) + divPaid,
                divPaid: divPaid,
                annualReturn: 0
            }

            return val;
        }
    }
}
</script>
<style scoped>
</style>