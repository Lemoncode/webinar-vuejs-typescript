<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()"/>
    <form-component @addTodo="addTodo"/>
    <info-component :amount-todo="todos.length"/>
    <todo-list-component :todos="reverseTodos" @selectTodo="toggleCompleted"/>
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
          id: 1,
          text: 'Learn Vue',
          completed: false,
        },
        {
          id: 2,
          text: 'Plan next trip',
          completed: true,
        },
        {
          id: 3,
          text: 'Go shopping',
          completed: false,
        },
      ] as ITodo[],
    };
  },
  computed: {
    reverseTodos(): ITodo[] {
      return this.todos.slice(0).reverse();
    },
  },
  methods: {
    addTodo(newTodo: string) {
      if (newTodo.length > 0) {
        this.todos.push({
          id: this.todos.length + 1,
          text: newTodo,
          completed: false,
        });
      }
    },
    toggleCompleted(todo: ITodo) {
      todo.completed = !todo.completed;
    },
  },
});
</script>
