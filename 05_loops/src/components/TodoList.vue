<template>
  <ul>
    <li v-for="todo in todos">{{ todo }}</li>
  </ul>
</template>

<script lang="ts">
import Vue from 'vue';
export default Vue.extend({
  name: 'TodoList',
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
});
</script>


