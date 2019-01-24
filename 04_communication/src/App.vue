<template>
  <div id="app">
    <header-component :header="header.toLocaleUpperCase()"/>
    <form-component :header="header" @onInput="header = $event"/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import HeaderComponent from './components/Header.vue';
import FormComponent from './components/Form.vue';

export default Vue.extend({
  name: 'app',
  components: {
    HeaderComponent,
    FormComponent,
  },
  data() {
    return {
      header: 'Todo list',
    };
  },
});
</script>

