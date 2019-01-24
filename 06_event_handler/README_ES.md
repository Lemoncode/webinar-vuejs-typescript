# Eventos de usuario

Vamos a permitir que el usuario agregue tareas nuevas desde el formulario.

- Primero crearemos en el componente `Form.vue` la propiedad donde se almacena `newTodo`
- Eliminaremos las props que no necesitamos
- Modificamos el input para que mediante un `v-model` utilice `newTodo`
- Agregamos un botón que emita un evento `addTodo` hacia el padre con `newTodo` como parámetro enviado

**src/components/Form.vue**

```diff
<template>
  <div class="add-todo-form">
-    <input 
-      type="text"
-      :value="header" 
-      @input="$emit('onInput', $event.target.value)"
-    >
+    <input type="text" v-model="newTodo" placeholder="Task text">
+    <button class="btn btn-primary" @click="$emit('addTodo', newTodo)">Add task</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Form',
-  props: {
-    header: {
-      type: String,
-      required: true,
-    },
-  },
+  data() {
+    return {
+      newTodo: '',
+    };
+  },
});
</script>

```

Ahora en el formulario padre, tenemos que quitar las props que le pasábamos y comenzar a escuchar el evento `addTodo` que nos enviará el hijo. Lo que el hijo nos envíe, lo agregaremos directamente a las tareas.

**src/App.vue**

```diff
<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()"/>
-    <form-component :header="header" @onInput="header = $event"/>
+    <form-component @addTodo="todos.push($event)"/>
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
});
</script>

```

## Eventos y modificadores

También nos interesaría que cuando el usuario pulsara `enter` después de escribir la tarea, se añadiera a nuestro listado de tareas. Veamos cómo hacerlo:

Vamos a engancharnos al `keyup` del usuario cuando está en nuestro *input*, utilizando el modificador `.enter`. De este modo cuando el usuario presione sólo la tecla *enter* se invocará nuestro código:

```diff
<template>
  <div class="add-todo-form">
-    <input type="text" v-model="newTodo" placeholder="Task text">
+    <input type="text" v-model="newTodo" @keyup.enter="$emit('addTodo', newTodo)" placeholder="Task text">
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

```

¿Sencillo verdad? Seguimos.

[Volver al índice](../README_ES.md/#agenda)
