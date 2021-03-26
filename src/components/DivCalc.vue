<template>
    <div>
        <h1 class="pl-4">Dividend Reinvestment Calculator (DRIP)</h1>
        <v-row class="pl-4 pt-2" no-gutters>
            <forms />
        </v-row>
    </div>
</template>
<script>
import forms from "@/components/Dividend/Form"
export default {
    components: {
        forms
    }
}
</script>
<style scoped>
</style>