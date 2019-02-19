<template>
  <div class="add-todo-form">
    <input
      type="text"
      v-model="newTodo"
      @keyup.enter="addTodo"
      placeholder="Task text"
      maxlength="200"
    >
    <p>{{ todoLength }}/200</p>
    <button class="btn btn-primary" :disabled="!newTodo.length" @click="addTodo">Add task</button>
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
  computed: {
    todoLength(): number {
      return this.newTodo.length;
    },
  },
  methods: {
    addTodo(): void {
      this.$emit('addTodo', this.newTodo);
      this.newTodo = '';
    },
  },
});
</script>
