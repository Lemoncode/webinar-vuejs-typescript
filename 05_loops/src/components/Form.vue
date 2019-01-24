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
