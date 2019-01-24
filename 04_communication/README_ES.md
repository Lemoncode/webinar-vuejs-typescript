# Comunicación entre componentes

Para mostrar cómo se lleva a cabo la comunicación entre componentes, vamos a empezar a fragmentar el ejemplo en componentes.

Primero, crearemos dentro de la carpeta `components` nuestro componente `Header.vue`

**src/components/Header.vue**

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

**src/App.vue**

```diff
<template>
  <div id="app">
-    <img alt="Lemoncode logo" src="./assets/logo_lemoncode.png">
+    <header-component />
    <h1>{{ header.toLocaleUpperCase() }}</h1>
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

> Los componentes los registramos en la sección `components` de Vue. Podemos también agregarlo con otro nombre, simplemente poniendo `'header-custom-component': HeaderComponent,` por ejemplo.

## Pasando datos mediante props

Ahora necesitamos pasar al nuevo componente el título a mostrar. Para ello, Vue te proporciona la comunicación mediante **props**.

Veamos cómo funciona:

Primero, en el componente `Header.vue` vamos a agregar una **props** de tipo `string` que esperará recibir del padre.

**src/components/Header.vue**

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

Y en el componente padre `App.vue` le pasamos la *prop* que espera el hijo, mediante el enlazado de atributos. Al final, las props son atributos HTML bindeados.

**src/App.vue**

```diff
<template>
  <div id="app">
-    <img alt="Lemoncode logo" src="./assets/logo_lemoncode.png">
-    <h1>{{ header.toLocaleUpperCase() }}</h1>
+    <header-component :header="header.toLocaleUpperCase()" />
    <input type="text" v-model="header">
  </div>
</template>
```

Lo bueno de tipar las **props** es que nos avisa por consola si estamos pasando un tipo que no es compatible.

Tenemos más opciones sobre las **props**, por ejemplo, podemos decir que la *props* es requerida, como es nuestro caso, así que vamos a modificarlo.

**src/components/Header.vue**

```html
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
    header: {
      type: String,
      required: true,
    },
  },
});
</script>

```

> Vemos como Vue nos ayuda a desarrollar más rápido ya que nos chiva todos los errores en consola rápidamente.

---

Sigamos componetizando nuestro ejemplo.

## Descomponer un v-model

Vamos a extraer nuestro `input` a otro componente para ver cómo interactuamos con él. El nuevo componente se llamará `Form.vue`

Es importante saber que los datos del padre no se deben editar directamente desde los componentes hijos. El flujo direccional siempre debe ser de arriba hacia abajo. Para evitar esto, no podemos usar directamente la directiva `v-model` desde el componente hijo con la props que recibimos. Sin embargo, vamos a ver qué realmente la directiva `v-model` no es más que azúcar sintáctico que nos provee Vue.

Internamente un `v-model` se descompone en un `value` y un evento `oninput`, así que podemos hacer lo siguiente:

- Recibimos `header` como props y la mostramos como el `value` del input.
- Cuando el usuario teclee algo, emitimos (`$emit`) hacia arriba un evento que deberá estar escuchando el padre.
  - El primer parámetro es el *nombre del evento que deberá estar escuchando el padre* con la directiva `v-on` (shorthand `@`)
  - El segundo parámetro es opcional y *sirve para enviar parámetros* que recibirá el padre.

**src/components/Form.vue**

```html
<template>
  <div class="add-todo-form">
    <input
      type="text"
      :value="header"
      @input="$emit('onInput', $event.target.value)"
    >
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Form',
  props: {
    header: {
      type: String,
      required: true,
    },
  },
});
</script>

```

Y en el componente padre:

**src/App.vue**

```diff
<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()" />
+    <form-component :header="header" @onInput="header = $event"/>
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

Más adelante veremos que existen otras formas más eficaces de controlar los eventos emitidos por los hijos mediante el envío de callbacks como props, por ejemplo.

[Volver al índice](../README.md/#agenda)
