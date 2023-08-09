<script setup>
import { ref, defineEmits } from 'vue'
import { categorias } from './../../datos.json';
const emit = defineEmits(['selectedConfig']);
const dificultad = ref("");
const categoria = ref("");
function onSubmit() {
  emit("selectedConfig", { categoria: categoria.value, dificultad: dificultad.value });
}
</script>

<template>
  <form @submit.prevent="onSubmit">
    <h1>TRIVIA GAME</h1>
    <select name="categoria" id="categoria" required v-model="categoria">
      <option value="" hidden>Seleccionar la categoria</option>
      <option v-for="categoria, index in categorias" :key="index" :value="index">{{ categoria }}</option>
    </select>
    <select name="dificultad" id="dificultad" required v-model="dificultad">
      <option value="" hidden>Seleccionar la dificultad</option>
      <option value="easy">Fácil</option>
      <option value="medium">Medio</option>
      <option value="hard">Dificil</option>
    </select>
    <button type="submit">Empezar</button>
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
