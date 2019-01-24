<template>
  <ul>
    <li
      v-for="todo in todos"
      @click="$emit('selectTodo', todo)"
      :class="{strikeout: todo.completed}"
    >{{ todo.text }}</li>
  </ul>
</template>

<script lang="ts">
import Vue from 'vue';
import { ITodo } from '../models/ITodo';

export default Vue.extend({
  name: 'TodoList',
  props: {
    todos: {
      type: Array as () => ITodo[],
      required: true,
    },
  },
});
</script>


