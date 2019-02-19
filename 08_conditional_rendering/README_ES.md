# Renderizado condicional

Ahora veamos cómo podemos controlar el renderizado de nuestras plantillas.

Imaginemos que queremos decirle al usuario que no tiene tareas pendientes. Con Vue esta tarea es muy sencilla, tan sólo debemos utilizar la directiva **v-if**.

Vamos a crear un componente `Info.vue` y gestionaremos el funcionamiento desde aquí:

### [src/components/Info.vue](./src/components/Info.vue)

```html
<template>
  <div class="info-todo">
    <span v-if="amountTodo === 0">Great! You don't have pending tasks</span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Info',
  props: {
    amountTodo: {
      type: Number,
      required: true,
    },
  },
});
</script>

```

Modificaremos el componente padre para pasarle la cantidad de tareas que tenemos:

### [src/App.vue](./src/App.vue)

```diff
<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()"/>
    <form-component @addTodo="addTodo"/>
+    <info-component :amount-todo="todos.length" />
    <todo-list-component :todos="todos"/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import HeaderComponent from './components/Header.vue';
import FormComponent from './components/Form.vue';
import TodoListComponent from './components/TodoList.vue';
+ import InfoComponent from './components/Info.vue';

export default Vue.extend({
  name: 'app',
  components: {
    HeaderComponent,
    FormComponent,
    TodoListComponent,
+    InfoComponent,
  },
  ···
```

Probemos eliminado desde la VueDevtools.

Si vemos el comportamiento de Vue al renderizar nuestras plantillas desde el inspector de HTML de nuestro navegador, vemos que directamente no aparece el código HTML. ¡Es fantástico!

Ademas de esta directiva, tenemos **v-else** y **v-else-if**. Veamos cómo funciona:

### [src/components/Info.vue](./src/components/Info.vue)

```diff
<template>
  <div class="info-todo">
    <span v-if="amountTodo === 0">Great! You don't have pending tasks</span>
+    <span v-else-if="amountTodo === 1">Uff...</span>
+    <span v-else>You have {{ amountTodo }} pending tasks</span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
export default Vue.extend({
  name: 'Info',
  props: {
    amountTodo: {
      type: Number,
      required: true,
    },
  },
});
</script>

```

También tenemos la directiva **v-show**, que tiene un comportamiento similar a **v-if**, sin embargo, sí renderiza nuestro HTML aunque aplica un `display:none` al elemento.

```html
  <h3 v-show="amountTodo === 0">I'm a ghost...</h3>
```

[Volver al índice](../README_ES.md/#agenda)
