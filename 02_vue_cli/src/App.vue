<template>
  <div id="app">
    <img alt="Lemoncode logo" src="./assets/logo_lemoncode.png">
    <h1>{{ header }}</h1>
    <input type="text" v-model="header">
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'app',
  data() {
    return {
      header: 'ToDo list',
    };
  },
});
</script>
