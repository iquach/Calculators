<template>
    <div>
        <h1 class="pl-4">Mortgage Calculator</h1>
        <v-row class="pl-4 pt-4">
            <v-col cols="4" >
                <forms v-on:input-data="getSeries"/>
            </v-col>
            <v-col cols="8">
                <chart :series="series"/>
            </v-col>
        </v-row>
    </div>
</template>
<script>
import Vue from 'vue'
import chart from '@/components/Mortage/Chart'
import forms from '@/components/Mortage/Form'
export default {
    components: {
        chart,
        forms
    },
    data() {
        return {
            series: [1756.23, 66, 329, 100]
        }
    },
    methods:{
        getSeries(val) {
            for(let i = 0; i < 4; i++) {
                Vue.set(this.series, i, val[i]);
            }
        }
    },
}
</script>
<style scoped>
</style>