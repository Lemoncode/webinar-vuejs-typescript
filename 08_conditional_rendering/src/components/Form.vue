<template>
  <div class="add-todo-form">
    <input type="text" v-model="newTodo" @keyup.enter="addTodo" placeholder="Task text">
    <button class="btn btn-primary" @click="addTodo">Add task</button>
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
  methods: {
    addTodo() {
      this.$emit('addTodo', this.newTodo);
      this.newTodo = '';
    },
  },
});
</script>
