# Bucles

En casi todas las aplicaciones solemos tener colecciones que debemos recorrer o filtrar. Vue nos proporciona la directiva `v-for` para realizar estas acciones de forma eficaz.

En nuestro componente principal `App.vue`, vamos a agregar en el `data` una colección de tareas a realizar, y luego mediante *props* se la pasaremos a un nuevo componente `TodoList.vue` para que renderice la lista de tareas mediante un `v-for`:

### [src/components/TodoList.vue](./src/components/TodoList.vue)

```html
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

```

### [src/App.vue](./src/App.vue)

<!--
todos: [
  'Learn Vue',
  'Plan next trip',
  'Go shopping',
],
-->

```diff
<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()"/>
    <form-component :header="header" @onInput="header = $event"/>
+    <todo-list-component :todos="todos"/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import HeaderComponent from './components/Header.vue';
import FormComponent from './components/Form.vue';
+ import TodoListComponent from './components/TodoList.vue';

export default Vue.extend({
  name: 'app',
  components: {
    HeaderComponent,
    FormComponent,
+    TodoListComponent,
  },
  data() {
    return {
      header: 'Todo list',
+      todos: [
+        'Learn Vue',
+        'Plan next trip',
+        'Go shopping',
+      ],
    };
  },
});
</script>

```

Ahora, si tenemos instaladas las VueDevtools, podemos desde consola agregar elementos mediante:

```
$vm0.todos.push("Nueva tarea")
```

O eliminarlos:

```
$vm0.todos.pop()
```

[Volver al índice](../README_ES.md/#agenda)
