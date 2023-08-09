<script setup>
import { ref, defineEmits, defineProps } from "vue";
import QuestionTrivia from "./QuestionTrivia.vue";
defineProps({
  results: Array,
});
const currentScreen = ref(1);
const score = ref(0);

function onResponse({ response }) {
  if (response == results[currentScreen.value - 1].correct_answer) {
    score.value++;
  }
}
function onSubmit() {
  currentScreen.value++;
}
</script>

<template>
  <form @submit.prevent="onSubmit">
    <div class="barra-container">
      <div
        v-for="index in results.length"
        :key="index"
        class="barra-bloque"
        :class="{ active: currentScreen >= index }"
        :data-index="index"
      ></div>
    </div>
    <QuestionTrivia
      v-for="(q, index) in results"
      :key="index"
      :category="q.category"
      :question="q.question"
      :correct-answer="q.correct_answer"
      :incorrect-answers="q.incorrect_answers"
      @selected-response="onResponse"
      v-show="index == currentScreen"
    />
    <button type="submit">Next</button>
  </form>
</template>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  row-gap: 1rem;
  align-items: center;
}
.barra-container {
  display: flex;
  width: 100%;
}
.barra-container .barra-bloque {
  flex: 1;
  height: 20px;
  background-color: #ccc;
}
.barra-container .barra-bloque.active {
  background-color: black;
}
select {
  width: 240px;
}
</style>
