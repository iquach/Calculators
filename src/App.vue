<template>
  <v-app>
    <v-app-bar
      flat
      app
      color="white"
    >
      <v-btn color="" to="/">Home</v-btn>
    </v-app-bar>
    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  components: {
  },

  data() {
    return {
    }
  },
};
</script>
<style scoped>
</style>