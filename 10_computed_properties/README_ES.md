# Propiedades computadas

Esta es una de las características que hacen grande a Vue. Nos permite transformar o realizar cálculos en nuestros datos y luego reutilizarlos fácilmente como una variable actualizada en nuestro template.

Las propiedades computadas son muy útiles y deben reemplazar expresiones complejas en nuestro template.

> Cuando declaremos propiedades computadas utilizando TypeScript, es necesario decirle qué retorna o de lo contrario fallará nuestro código.

Por ejemplo, vamos a mostrar el número de caracteres que va tecleando el usuario. Primero creamos el método:

**src/components/Form.vue**

```diff
  },
+  computed: {
+    todoLength(): number {
+      return this.newTodo.length;
+    },
+  },
  },
  methods: {
```

Y ahora lo usaremos en nuestro template:

**src/components/Form.vue**

```diff
<template>
  <div class="add-todo-form">
    <input
      type="text"
      v-model="newTodo"
      @keyup.enter="addTodo"
      placeholder="Task text"
+      maxlength="200"
    >
+    <p>{{ todoLength }}/200</p>
    <button class="btn btn-primary" :disabled="!newTodo.length" @click="addTodo">Add task</button>
  </div>
</template>
```

Genial, ¿verdad?

Ahora vamos a ver cómo podemos utilizarlo con otras directivas de Vue. En este caso, ordenaremos nuestras tareas para que las nuevas tareas estén al principio (que sí, que podemos hacerlo con un `unshift` pero entonces no aprenderíamos cosillas interesantes).

Primero, creamos el método:

**src/App.vue**

```diff
 computed: {
    todoLength(): number {
      return this.newTodo.length;
    },
+    reverseTodos(): ITodo[] {
+      return this.todos.slice(0).reverse();
+    },
```

Y ahora en vez de pasarle `todo` como *props* al hijo, le pasaremos `reverseTodos`:

```diff
<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()"/>
    <form-component @addTodo="addTodo"/>
    <info-component :amount-todo="todos.length"/>
-    <todo-list-component :todos="todos" @selectTodo="toggleCompleted"/>
+    <todo-list-component :todos="reverseTodos" @selectTodo="toggleCompleted"/>
  </div>
</template>
```

Veamos cómo las tareas se ordenan ahora.

---

> Ahora es buen momento para entender por qué es necesario decirle al **v-for** cuál es la clave de cada item.

Si nos fijamos en nuestras tareas, cuando agregamos una nueva podemos ver cómo se superponen las clases, subrayando el elemento que no corresponde. Esto es debido a que no le estamos diciendo a Vue un elemento fundamental para tratar los bucles. La **KEY** de cada elemento.

Veamos cómo se comporta si agregamos una clave única para cada elemento.

- Primero modificamos el modelo:

**src/models/ITodo.ts**

```diff
export interface ITodo {
+  id: number;
  text: string;
  completed: boolean;
}
```

**src/App.vue**

```diff
  ···
  data() {
    return {
      header: 'Todo list',
      todos: [
        {
+          id: 1,
          text: 'Learn Vue',
          completed: false,
        },
        {
+          id: 2,
          text: 'Plan next trip',
          completed: true,
        },
        {
+          id: 3,
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
      this.todos.push({
+        id: this.todos.length + 1,
        text: newTodo,
        completed: false,
      });
    },
  ···
```

**src/components/TodoList.vue**

```diff
<template>
  <ul>
    <li
      v-for="todo in todos"
+      :key="todo.id"
      @click="$emit('selectTodo', todo)"
      :class="{strikeout: todo.completed}"
    >{{ todo.text }}</li>
  </ul>
</template>
```

[Volver al índice](../README.md/#agenda)
