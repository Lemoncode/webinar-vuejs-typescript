<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()"/>
    <form-component @addTodo="addTodo"/>
    <info-component :amount-todo="todos.length"/>
    <todo-list-component :todos="todos" @selectTodo="toggleCompleted"/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import HeaderComponent from './components/Header.vue';
import FormComponent from './components/Form.vue';
import TodoListComponent from './components/TodoList.vue';
import InfoComponent from './components/Info.vue';
import { ITodo } from './models/ITodo';

export default Vue.extend({
  name: 'app',
  components: {
    HeaderComponent,
    FormComponent,
    TodoListComponent,
    InfoComponent,
  },
  data() {
    return {
      header: 'Todo list',
      todos: [
        {
          text: 'Learn Vue',
          completed: false,
        },
        {
          text: 'Plan next trip',
          completed: true,
        },
        {
          text: 'Go shopping',
          completed: false,
        },
      ] as ITodo[],
    };
  },
  methods: {
    addTodo(newTodo: string) {
      this.todos.push({
        text: newTodo,
        completed: false,
      });
    },
    toggleCompleted(todo: ITodo) {
      todo.completed = !todo.completed;
    },
  },
});
</script>
