<template>
  <div>
    <img alt="Lemoncode logo" src="../assets/logo_lemoncode.png">
    <h1>{{ header }}</h1>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Header',
  props: {
    header: {
      type: String,
      required: true,
    },
  },
});
</script>
