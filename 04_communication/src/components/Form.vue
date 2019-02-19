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

<style scoped>
.add-todo-form {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.add-todo-form input {
  width: 70%;
  border-radius: 3px;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);
  border: 1px solid #f1f5f8;
  color: #606f7b;
  padding: 0.5rem 0.75rem;
  box-sizing: border-box;
  font-size: 1rem;
  letter-spacing: 0.5px;
  margin: 0.5rem 0;
}
</style>
