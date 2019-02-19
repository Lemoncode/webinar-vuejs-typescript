<template>
  <div class="add-todo-form">
    <input 
      type="text"
      :value="value" 
      @input="($event) => $emit('input', $event.target.value)"
    >
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
