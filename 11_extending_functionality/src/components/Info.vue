<template>
  <div class="info-todo">
    <span v-if="amountTodo === 0">Great! You don't have pending tasks</span>
    <span v-else>You have {{ amountTodo }} pending {{ amountTodo | pluralize }}</span>
    <ul class="filters">
      <li @click="onClick('all')" :class="{ selected: visibility === 'all' }">All</li>
      <li @click="onClick('active')" :class="{ selected: visibility === 'active' }">Active</li>
      <li @click="onClick('completed')" :class="{ selected: visibility === 'completed' }">Complete</li>
    </ul>    
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
export default Vue.extend({
  name: 'Info',
  filters: {
    pluralize(count: number) {
      return count === 1 ? 'task' : 'tasks';
    },
  },
  props: {
    amountTodo: {
      type: Number,
      required: true,
    },
    visibility: {
      type: String,
      required: true,
    },
  },
  methods: {
    onClick(type: string) {
      this.$emit('selectVisibility', type);
    },
  },
});
</script>

