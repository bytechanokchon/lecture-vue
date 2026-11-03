<script setup>
import { ref } from 'vue';

const message = ref('Hello World from Vue 3!');
const dynamicId = ref(null);
const isButtonDisabled = ref(true)
</script>

<template>
  <p>{{ message }}</p>

  <div v-bind:id="dynamicId">
    Dynamic ID: {{ dynamicId }}
  </div>

  <button :disabled="isButtonDisabled">
    Submit
  </button>
</template>

<style scoped></style>
