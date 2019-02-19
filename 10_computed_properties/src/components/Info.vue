<template>
  <div class="info-todo">
    <span v-if="amountTodo === 0">Great! You don't have pending tasks</span>
    <span v-else-if="amountTodo === 1">Uff...</span>
    <span v-else>You have {{ amountTodo }} pending tasks</span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Info',
  props: {
    amountTodo: {
      type: Number,
      required: true,
    },
  },
});
</script>
