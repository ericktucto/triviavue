<script setup>
import { ref, computed, defineEmits, defineProps, onUnmounted } from "vue";
import QuestionTrivia from "./QuestionTrivia.vue";
const props = defineProps({
  results: Array,
  modelValue: Number,
});
const emit = defineEmits(["terminatedTrivia", "update:modelValue"]);
const currentScreen = ref(1);
const response = ref("");
const barra = ref(null);
const score = computed({
  get() {
    return props.modelValue
  },
  set(value) {
    emit('update:modelValue', value)
  }
})
const segundos = ref(0);
function update() {
  if (segundos.value <= 10) {
    segundos.value++;
  }
  if (segundos.value == 10) {
    onSubmit();
  }
}
setInterval(update, 900)


function onSubmit() {
  if (response.value == props.results[currentScreen.value]?.correct_answer) {
    score.value = score.value + 1;
  }
  // next questions
  currentScreen.value++;
  segundos.value = 0;
  // check if all questions are answered
  if (currentScreen.value == props.results.length) {
    clearInterval(update);
    emit("terminatedTrivia");
  }
}
onUnmounted(() => clearInterval(update));
</script>

<template>
  <form @submit.prevent="onSubmit">
    <div class="barra-container">
      <div>{{ segundos }} s</div>
      <div
        class="barra-bloque"
      >
        <div ref="barra" class="tiempo" :style="{ width: `${segundos * 10}%` }"></div>
      </div>
    </div>
    <QuestionTrivia
      v-for="(q, index) in results"
      :key="index"
      :category="q.category"
      :question="q.question"
      :correct-answer="q.correct_answer"
      :incorrect-answers="q.incorrect_answers"
      v-model="response"
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
  column-gap: 12px;
  align-items: center;
}
.barra-container .barra-bloque {
  flex: 1;
  height: 20px;
  background-color: #ccc;
  display: flex;
}
.barra-container .barra-bloque .tiempo {
  background-color: black;
}
</style>
