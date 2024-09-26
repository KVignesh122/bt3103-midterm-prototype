<template>
  <v-app>
    <v-main class="app-container">
      <GPTClient />
    </v-main>
  </v-app>
</template>

<script>
import GPTClient from './components/GPTClient.vue';
export default {
  name: 'App',
};
</script>

<style scoped>
/* Ensure the background matches the theme */
.app-container {
  background-color: #F7F8F3;
  min-height: 100vh;
  padding: 20px;
}

/* General body styling */
body {
  font-family: 'Arial, sans-serif';
  color: #4A4A4A;
  background-color: #F7F8F3;
}
</style>
