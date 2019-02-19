# Comunicación entre componentes

Para mostrar cómo se lleva a cabo la comunicación entre componentes, vamos a empezar a fragmentar nuestro ejemplo.

Primero, crearemos dentro de la carpeta `components` nuestro componente `Header.vue`

### [src/components/Header.vue](./src/components/Header.vue)

```html
<template>
  <div>
    <img alt="Lemoncode logo" src="../assets/logo_lemoncode.png">
    <h1>HEADER</h1>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Header',
});
</script>

```

Y ahora en nuestro archivo `App.vue` vamos a importar el componente y utilizarlo:

### [src/App.vue](./src/App.vue)

```diff
<template>
  <div id="app">
-    <img alt="Lemoncode logo" src="./assets/logo_lemoncode.png">
-    <h1>{{ header.toLocaleUpperCase() }}</h1>
+    <header-component />
    <input type="text" v-model="header">
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
+ import HeaderComponent from './components/Header.vue';

export default Vue.extend({
  name: 'app',
+  components: {
+    HeaderComponent,
+  },
  data() {
    return {
      header: 'ToDo list',
    };
  },
});
</script>
```

> Los componentes los registramos en la sección `components` de Vue. Podemos también agregarlo con otro nombre, por ejemplo:

```html
components: {
  'header-custom-component': HeaderComponent,
},
```

## Pasando datos mediante props

Ahora necesitamos pasar al nuevo componente el título a mostrar. Para ello, Vue te proporciona la comunicación mediante **props**, respetando el flujo unidireccional de padre a hijo.

Veamos cómo funciona:

Primero, en el componente `Header.vue` vamos a agregar una **props** de tipo `string` que esperará recibir del padre.

### [src/components/Header.vue](./src/components/Header.vue)

```diff
<template>
  <div>
    <img alt="Lemoncode logo" src="../assets/logo_lemoncode.png">
-    <h1>HEADER</h1>
+    <h1>{{ header }}</h1>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Header',
+  props: {
+    header: String,
+  },
});
</script>

```

Y en el componente padre `App.vue` le pasamos la *prop* que espera el hijo, mediante el enlazado de atributos.

### [src/App.vue](./src/App.vue)

```diff
<template>
  <div id="app">
-    <header-component />
+    <header-component :header="header.toLocaleUpperCase()" />
    <input type="text" v-model="header">
  </div>
</template>
```

Lo bueno de tipar las **props** es que nos avisa por consola si estamos pasando un tipo que no es compatible. Por ejemplo:

```diff
- <header-component :header="header.toLocaleUpperCase()" />
+ <header-component :header="1" />
```

Tenemos más opciones sobre las **props**. Podemos decir que la *prop* es requerida, como es nuestro caso, así que vamos a modificarlo.

### [src/components/Header.vue](./src/components/Header.vue)

```diff
<template>
  <div>
    <img alt="Lemoncode logo" src="../assets/logo_lemoncode.png">
    <h1>{{ header }}</h1>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Header',
  props: {
-    header: String,
+    header: {
+      type: String,
+      required: true,
+    },
  },
});
</script>

```

> Vemos como Vue nos ayuda a desarrollar más rápido ya que nos chiva todos los errores en consola rápidamente.

```diff
- <header-component :header="header.toLocaleUpperCase()" />
+ <header-component />
```

Sigamos componetizando nuestro ejemplo.

## Descomponer un v-model

Es importante saber que los datos del padre no se deben editar directamente desde los componentes hijos. El flujo direccional siempre debe ser de arriba hacia abajo. Para evitar esto, no podemos usar directamente la directiva `v-model` desde el componente hijo con la **prop** que recibimos. Sin embargo, vamos a ver cómo podemos utilizar el azúcar sintáctico que nos aporta la directiva `v-model` para poder usarlo:

<!-- Slide -->

Internamente un `v-model` se descompone en un `value` y un evento que escucha un `input`, así que vamos a extraer nuestro `input` a otro componente para ver cómo podemos interactuar con él. El nuevo componente se llamará `Form.vue`

- Recibimos el value de `header` como props y lo mostramos como el `value` del input.
- Cuando el usuario teclee algo, emitimos (`$emit`) hacia arriba un evento `input` que escuchará el padre mediante el v-model.
  - El primer parámetro es el *nombre del evento que deberá estar escuchando el padre* con la directiva `v-on` (shorthand `@`)
  - El segundo parámetro es opcional y *sirve para enviar parámetros* que recibirá el padre.

### [src/components/Form.vue](./src/components/Form.vue)

```html
<template>
  <div class="add-todo-form">
    <input
      type="text"
      :value="value"
      @input="($event) => this.$emit('input', $event.target.value)"
    />
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Form',
  props: {
    value: String,
  },
});
</script>

```

Y en el componente padre:

### [src/App.vue](./src/App.vue)

```diff
<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()" />
+    <form-component :header="header" @onInput="header = $event" />
-    <input type="text" v-model="header">
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import HeaderComponent from './components/Header.vue';
+ import FormComponent from './components/Form.vue';

export default Vue.extend({
  name: 'app',
  components: {
    HeaderComponent,
+    FormComponent,
  },
  data() {
    return {
      header: 'Todo list',
    };
  },
});
</script>
```

Más adelante veremos que existen otras formas más eficaces de controlar los eventos emitidos por los hijos mediante el envío de `callbacks` como props, por ejemplo.

[Volver al índice](../README_ES.md/#agenda)
