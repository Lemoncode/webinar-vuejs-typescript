# Vue CLI

Pero si queremos montar nuestra aplicación para sacar el máximo partido a Vue, lo mejor es hacerlo mediante la CLI que nos proporciona el framework.

Primero la instalamos de forma global:

```
npm install -g @vue/cli
```

Y seguidamente, creamos nuestro proyecto:

```
vue create todo-app
```

Nosotros vamos a meternos directamente con lo duro así que crearemos el proyecto seleccionando la configuración para poder agregar TypeScript.

Una vez ha finalizado, ya tenemos nuestra app lista para funcionar: `npm run serve`

Vamos a eliminar algunas cosas que nos han creado por defecto como el componente `HelloWorld` y el contenido del div `app`.

```html
<template>
  <div id="app"></div>
</template>

<script lang="ts">
  import Vue from "vue";

  export default Vue.extend({
    name: "app",
  });
</script>

```

Y ahora vamos a agregar nuestros datos a la aplicación al igual que hicimos en el ejemplo anterior:

```html
<template>
  <div id="app">
    <h1>{{ header }}</h1>
    <input type="text" v-model="header">
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'app',
  data() {
    return {
      header: 'ToDo list',
    };
  },
});
</script>

```

[Volver al índice](../README_ES.md/#agenda)
