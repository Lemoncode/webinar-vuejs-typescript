# Enlaces de atributo y clases dinámicas

## Enlaces de atributo

Vue tiene una característica genial que es el enlace de atributos del DOM. Mediante la directiva **v-bind** podemos enlazar una propiedad de nuestro HTML a una función o propiedad de Vue.

Por ejemplo, vamos a engancharnos al atributo `disabled` de nuestro botón, para que el usuario no pueda agregar elementos sin texto:

```diff
<template>
  <div class="add-todo-form">
    <input type="text" v-model="newTodo" @keyup.enter="addTodo" placeholder="Task text">
-    <button class="btn btn-primary" @click="addTodo">Add task</button>
+    <button class="btn btn-primary" :disabled="!newTodo.length" @click="addTodo">Add task</button>
  </div>
</template>
  ···
```

> Recuerda que `:disabled="!newTodo.length"` es el shorthand de `v-bind:disabled="!newTodo.length"`

## Clases y estilos dinámicos

Es muy útil enlazarnos al estilado de nuestros componentes. Podemos hacerlo con las clases o con los estilos en línea mediante las dos formas que provee Vue:

- sintaxis de objeto.
- sintaxis de array.

Veremos cómo darle un estilo a las tareas que ya han sido completadas. Para ello, necesitamos editar nuestro objeto **data**, y ya que estamos con TypeScript, vamos a tiparlo:

### Creando una interfaz para tipar los datos

Primero creamos una carpeta en el raíz que se llamará `models`. Dentro de esta carpeta crearemos una interfaz `ITodo.ts` para usarla como modelo en nuestros componentes.

**src/models/ITodo.ts**

```ts
export interface ITodo {
  text: string;
  completed: boolean;
}

```

Ahora en `App.vue` usamos la interfaz:

**src/App.vue**

```diff
<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()"/>
    <form-component @addTodo="addTodo"/>
    <info-component :amount-todo="todos.length" />
    <todo-list-component :todos="todos"/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import HeaderComponent from './components/Header.vue';
import FormComponent from './components/Form.vue';
import TodoListComponent from './components/TodoList.vue';
import InfoComponent from './components/Info.vue';
+ import { ITodo } from "./models/ITodo";

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
-        'Learn Vue',
-        'Plan next trip',
-        'Go shopping',
-      ],
+        { text: 'Learn Vue', completed: false },
+        { text: 'Plan next trip', completed: true },
+        { text: 'Go shopping', completed: false },
+      ] as ITodo[],
    };
  },
  methods: {
    addTodo(newTodo: string) {
-      this.todos.push(newTodo);
+      this.todos.push({
+        text: newTodo,
+        completed: false,
+      });
    },
  },
});
</script>

```

Y por último, editamos el componente que renderiza nuestras tareas `TodoList.vue`:

**src/componets/TodoList.vue**

```diff
<template>
  <ul>
-    <li v-for="todo in todos">{{ todo }}</li>
+    <li v-for="todo in todos">{{ todo.text }}</li>
  </ul>
</template>

<script lang="ts">
import Vue from 'vue';
+ import { ITodo } from "../models/ITodo";

export default Vue.extend({
  name: 'TodoList',
  props: {
    todos: {
-      type: Array,
+      type: Array as () => ITodo[],
      required: true,
    },
  },
});
</script>

```

### Sintaxis de objetos

Podemos pasar un objeto para `:class` y alternar de forma dinámica las clases:

**src/components/TodoList.vue**

```diff
  <ul>
-    <li v-for="todo in todos">{{ todo.text }}</li>
+    <li v-for="todo in todos" :class="{strikeout: todo.completed}">{{ todo.text }}</li>
  </ul>
```

### Sintaxis de array

La sintaxis de array nos permite más flexibilidad, pero tal vez pueda resultar menos legible:

```diff
  <ul>
-    <li v-for="todo in todos">{{ todo.text }}</li>
+    <li v-for="todo in todos" :class="[todo.completed ? 'strikeout' : '']">{{ todo.text }}</li>
  </ul>
```

Pero de esta forma podemos alternar entre distintas clases, no sólo activarlas. Vamos a agregar otra condición para otra propiedad:

**src/models/ITodo.ts**

```diff
interface ITodo {
  text: string;
  completed: boolean;
+  highPriority: boolean;
}
```

**src/App.vue**

_Sustituimos directamente_

```js
  todos: [
    {
      text: 'Learn Vue',
      completed: false,
      highPriority: false,
    },
    {
      text: 'Plan next trip',
      completed: true,
      highPriority: false,
    },
    {
      text: 'Go shopping',
      completed: false,
      highPriority: true,
    },
  ] as ITodo[],
```

```diff
  ···
  methods: {
    addTodo() {
      this.todos.push({
        text: this.newTodo,
        completed: false,
+        highPriority: false,
      });
      this.newTodo = '';
    },
  },
});
</script>
```

Y ahora en el template agregamos la nueva condición:

**src/components/TodoList.vue**

```diff
  <ul>
-    <li v-for="todo in todos" :class="[todo.completed ? 'strikeout' : '']">{{ todo.text }}</li>
+    <li v-for="todo in todos" :class="[todo.completed ? 'strikeout' : '', todo.highPriority ? 'priority' : '']">{{ todo.text }}</li>
  </ul>
```

¡Genial! Ahora, vamos a enganchar un método de *toggle* para que alterne entre completada y no completada.

Primero, vamos a emitir hacia el padre un evento con la tarea seleccionada cuando el usuario haga clic en ella:

**src/components/TodoList.vue**

```html
<template>
  <ul>
    <li
      v-for="todo in todos"
      @click="$emit('selectTodo', todo)"
      :class="[todo.completed ? 'strikeout' : '', todo.highPriority ? 'priority' : '']"
    >{{ todo.text }}</li>
  </ul>
</template>
```

Y ahora en el padre, controlaremos el *toggle* entre completada y no completada:

**src/App.vue**

```diff
  methods: {
    addTodo() {
      this.todos.push({
        text: this.newTodo,
        completed: false,
        highPriority: false,
      });
      this.newTodo = '';
    },
+    toggleCompleted(todo: ITodo) {
+      todo.completed = !todo.completed;
+    },
  },
```

Con la reactividad de Vue al hacer clic sobre la tarea vemos cómo cambia automáticamente. ¿Esto es genial verdad?

Bien, vamos a deshacer algunas cosas para dejarlo listo.

**src/models/ITodo.ts**

```ts
export interface ITodo {
  text: string;
  completed: boolean;
}
```

**src/components/TodoList.vue**

```html
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
```

**src/App.vue**

```html
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

```

[Volver al índice](../README.md/#agenda)
