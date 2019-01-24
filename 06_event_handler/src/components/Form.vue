<template>
  <div class="add-todo-form">
    <input type="text" v-model="newTodo" @keyup.enter="$emit('addTodo', newTodo)" placeholder="Task text">
    <button class="btn btn-primary" @click="$emit('addTodo', newTodo)">Add task</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Form',
  data() {
    return {
      newTodo: '',
    };
  },
});
</script>
