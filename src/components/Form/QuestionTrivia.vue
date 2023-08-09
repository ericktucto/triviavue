<script setup>
import { ref, computed, defineEmits, defineProps } from "vue";
const emit = defineEmits(["selectedResponse", "update:modelValue"]);
const props = defineProps({
  question: String,
  category: String,
  correctAnswer: String,
  incorrectAnswers: Array,
  modelValue: String,
});
const answers = ref(
  props.incorrectAnswers
    .concat([props.correctAnswer])
    .sort(() => Math.random() - 0.5)
);
const response = computed({
  get() {
    return props.modelValue
  },
  set(value) {
    emit('update:modelValue', value)
  }
})
const name = ref(props.question.replace(/[^a-z0-9]/gi, "-"));
function parse(text) {
  return decodeURIComponent(text);
}
</script>

<template>
  <div>
    <h2>{{ parse(category) }}</h2>
    <h3>{{ parse(question) }}</h3>
    <div class="answers-container">
      <label
        v-for="(answer, index) in answers"
        :key="index"
        :class="{ 'btn-outline': response != answer, btn: true }"
      >
        <input
          type="radio"
          :value="answer"
          :name="name"
          v-model="response"
        />
        {{ parse(answer)  }}
      </label>
    </div>
  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
  row-gap: 1rem;
  align-items: center;
}
h2,
h3 {
  margin: 0;
}
.answers-container {
  display: flex;
  flex-direction: row;
  white-space: nowrap;
  column-gap: 1rem;
  flex-wrap: wrap;
  justify-content: center;
}
.answers-container .btn {
  border-radius: 2rem;
}
.answers-container input {
  display: none;
}
</style>
