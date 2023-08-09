<script setup>
import { ref, defineEmits, defineProps } from "vue";
const props = defineProps({
  score: 10,
});
</script>

<template>
  <form>
    <div>
      <img src="/trophy.png" alt="trophy" />
    </div>
    <h2>Congratulations, your score is {{ score }} point(s).</h2>
    <div class="buttons-container">
      <button type="button" @click="$emit('restart')">Restart</button>
      <button type="button" class="btn-outline">Share</button>
    </div>
  </form>
</template>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  row-gap: 1rem;
  align-items: center;
}
.buttons-container {
  display: flex;
  width: 100%;
  justify-content: center;
  column-gap: 12px;
}
</style>
