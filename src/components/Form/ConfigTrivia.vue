<script setup>
import { ref, defineEmits } from 'vue'
import { categories } from './../../datos.json';
const emit = defineEmits(['selectedConfig']);
const difficulty = ref("easy");
const category = ref("10");
function onSubmit() {
  emit("selectedConfig", { category: category.value, difficulty: difficulty.value });
}
</script>

<template>
  <form @submit.prevent="onSubmit">
    <h1>TRIVIA GAME</h1>
    <select name="category" id="category" required v-model="category">
      <option value="" hidden>Select category</option>
      <option v-for="c, index in categories" :key="index" :value="index">{{ c }}</option>
    </select>
    <select name="difficulty" id="difficulty" required v-model="difficulty">
      <option value="" hidden>Select difficulty</option>
      <option value="easy">Easy</option>
      <option value="medium">Medium</option>
      <option value="hard">Hard</option>
    </select>
    <button type="submit">Start</button>
  </form>
</template>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  row-gap: 1rem;
  align-items: center;
}
select {
    width: 240px;
}

</style>
