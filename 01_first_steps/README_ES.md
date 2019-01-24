# Primeros pasos con Vue.js

Vamos a comprobar lo fácil que es integrar Vue desde un CDN a nuestro sitio. Seguidamente, veremos cómo es el sistema de reactividad que nos proporciona Vue.

Primero partiremos de una plantilla HTML básica en la que añadiremos Vue desde un CDN.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>ToDo list</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <!-- development version, includes helpful console warnings -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </body>
</html>

```

Seguidamente, agregaremos un contenedor `div` que será donde conviva nuestra aplicación y añadiremos una etiqueta `h1`.

```diff
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>ToDo list</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
+    <div id="app">
+      <h1>Hello Vue!</h1>
+    </div>
    <!-- development version, includes helpful console warnings -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </body>
</html>

```

Con esto ya tenemos una aplicación Vue funcionando en nuestro sitio.

Para empezar a hacer cosillas, vamos a crear una instancia de Vue. Para ello, agregaremos una etiqueta `script` y le diremos a Vue cierta información que necesita para ejecutarse, como por ejemplo, a qué elemento del DOM debe engancharse. En nuestro caso, será al `div` cuyo id es `app`.

```diff
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>ToDo list</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div id="app">
      <h1>Hello Vue!</h1>
    </div>
    <!-- development version, includes helpful console warnings -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
+    <script>
+      new Vue({
+        el: "#app",
+      });
+    </script>
  </body>
</html>

```

Ahora agregaremos datos a nuestra aplicación y eso será tan sencillo como agregar un objeto al data que espera Vue.

```diff
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>ToDo list</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div id="app">
      <h1>Hello Vue!</h1>
    </div>
    <!-- development version, includes helpful console warnings -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      new Vue({
        el: "#app",
+        data: {
+          header: "Vue is awesome!",
+        },
      });
    </script>
  </body>
</html>

```

En la propiedad `data` será donde almacenaremos los datos de nuestra instancia de Vue.

Vamos a ver cómo podemos usar nuestros datos en la aplicación. Vue utiliza una sintaxis de plantilla llamada `sintaxis de doble bigote`. Sólo cambiemos el contenido del `h1` por nuestro header y tachán!

Ya tenemos nuestro header preparado y reactivo para funcionar.

Una de las características más importantes de Vue es su sistema de reactividad. Veamos cómo funciona.

Vamos a crear una caja de texto donde el usuario podrá escribir y lo sincronizaremos con nuestro header. Para ello, haremos uso de la directiva `v-model`.

```diff
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>ToDo list</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div id="app">
      <h1>Hello Vue!</h1>
+      <input type="text" v-model="header" />
    </div>
    <!-- development version, includes helpful console warnings -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      new Vue({
        el: "#app",
        data: {
          header: "Vue is awesome!",
        },
      });
    </script>
  </body>
</html>

```

Ahora al teclear vemos cómo se actualiza automáticamente el header de nuestra página. ¡Perfecto!

Pero veamos un poco más sobre la reactividad de Vue. Vamos a asignar la instancia de Vue a una variable para poder cambiar desde consola nuestro header y ver qué ocurre.

```diff
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>ToDo list</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div id="app">
      <h1>{{ header }}</h1>
      <input type="text" v-model="header" />
    </div>
    <!-- development version, includes helpful console warnings -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
-      new Vue({
+      var app = new Vue({
        el: "#app",
        data: {
          header: "Vue is awesome!",
        },
      });
    </script>
  </body>
</html>

```

> Abrimos las herramientas de desarrollador y mostramos la consola.

Ahora, si llamamos a nuestra variable, veremos que nos muestra el objeto que contiene nuestra instancia de Vue.

Si tecleamos `app.$data.header = "Testing from the console"`, veremos como cambia instantáneamente el contenido de nuestro header y del input.

Y después de esta introducción, vamos a meternos en el barro...

[Volver al índice](../README_ES.md/#agenda)
