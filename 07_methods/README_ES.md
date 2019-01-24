# Métodos Vue

Ejecutar JavaScript directamente en las plantillas o en las directivas no es recomendable. Vamos a ver cómo podemos extraer nuestra funcionalidad a los métodos de Vue.

En nuestro componente `Form.vue` vamos a extraer la función de emitir las nuevas taras hacia el padre:

**src/components/Form.vue**

```html
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
    },
  },
});
</script>

```

Aprovechamos para dejar la caja de texto vacía una vez se ha enviado una nueva tarea:

```diff
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
+      this.newTodo = '';
    },
  },
});
</script>

```

Moviendo nuestra lógica desde las plantillas a los métodos de Vue estamos mejorando nuestra aplicación porque ahora podemos refactorizar mejor nuestro código, testearlo y depurarlo con facilidad.

Vamos a hacer lo mismo en el componente padre:

**src/App.vue**

```diff
<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()"/>
-    <form-component @addTodo="todos.push($event)"/>
+    <form-component @addTodo="addTodo"/>
    <todo-list-component :todos="todos"/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import HeaderComponent from './components/Header.vue';
import FormComponent from './components/Form.vue';
import TodoListComponent from './components/TodoList.vue';

export default Vue.extend({
  name: 'app',
  components: {
    HeaderComponent,
    FormComponent,
    TodoListComponent,
  },
  data() {
    return {
      header: 'Todo list',
      todos: [
        'Learn Vue',
        'Plan next trip',
        'Go shopping',
      ],
    };
  },
+  methods: {
+    addTodo(newTodo: string) {
+      this.todos.push(newTodo);
+    },
+  },
});
</script>

```

[Volver al índice](../README_ES.md/#agenda)
