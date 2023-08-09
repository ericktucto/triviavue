<script setup>
import { ref, defineEmits, defineProps } from "vue";
const emit = defineEmits(["selectedResponse"]);
const props = defineProps({
  question: String,
  category: String,
  correctAnswer: String,
  incorrectAnswers: Array,
});
const answers = ref(
  props.incorrectAnswers
    .concat([props.correctAnswer])
    .sort(() => Math.random() - 0.5)
);
const response = ref("");
const name = ref(props.question.replace(/[^a-z0-9]/gi, "-"));
function onChange(answer) {
  console.log(response.value == answer)
  emit("selectedResponse", { response: response.value });
}
</script>

<template>
  <div>
    <h2>{{ props.category }}</h2>
    <h3>{{ props.question }}</h3>
    <div class="answers-container">
      <label
        v-for="(answer, index) in answers"
        :key="index"
        :class="{ unselected: response != answer, btn: true }"
      >
        <input
          type="radio"
          :value="answer"
          @change="onChange(answer)"
          :name="name.value"
          v-model="response"
        />
        <span v-html="answer"></span>
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
}
.answers-container .btn {
  border-radius: 2rem;
}
.answers-container .btn.unselected {
  border: #ccc solid 1px;
  background-color: white;
  color: black;
}
.answers-container input {
  display: none;
}
</style>
