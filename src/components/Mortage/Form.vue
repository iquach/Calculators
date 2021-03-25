<template>
    <div>
        {{series}}
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
    </div>    
</template>
<script>
import Vue from 'vue'
export default {
    data() {
        return {
            price: 500000,
            down: 100000,
            downPercentage: 20,
            displayDownPercentage: 20,
            loanLength: 30,
            loanLengths: [40, 30, 25, 20, 15, 10],
            interest: 3.32,
            insurance: 66,
            tax: 329,
            hoa: 100,
            changeFlag: false,
            series:  [1756.23, 66, 329, 100]
        }
    },
    methods:{
        emit(index, val) {
            if(val == "") {
                val = 0;
            }
            Vue.set(this.series, index, val);
            this.$emit('input-data', this.series);
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
            this.emit(0, val);
        },
        insurance: function(val) {
            this.emit(1, val);
        },
        tax: function(val) {
            this.emit(2, val);
        },
        hoa: function(val) {
            this.emit(3, val);
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