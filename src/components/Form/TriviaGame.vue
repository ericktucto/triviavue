<script setup>
import { ref, defineEmits, defineProps } from "vue";
import QuestionTrivia from "./QuestionTrivia.vue";
defineProps({
  results: Array,
});
</script>

<template>
  <form>
    <QuestionTrivia
      v-for="q, index in results"
      :key="index"
      :category="q.category"
      :question="q.question"
      :correct-answer="q.correct_answer"
      :incorrect-answers="q.incorrect_answers"
    />
    <button type="submit">Next</button>
  </form>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
  row-gap: 1rem;
  align-items: center;
}
select {
  width: 240px;
}
</style>
