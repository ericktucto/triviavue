<script setup>
import { ref } from "vue";
import ConfigTrivia from "./components/Form/ConfigTrivia.vue";
import TriviaGame from "./components/Form/TriviaGame.vue";

const results = ref([]);
const currentScreen = ref("config");

async function requestQuestions({ category, difficulty }) {
  let url =
    "https://opentdb.com/api.php?amount=5&category=:category&difficulty=:difficulty&type=multiple";
  url = url.replace(":category", category).replace(":difficulty", difficulty);
  const response = await fetch(url);
  const data = await response.json();
  data.results.forEach(r => results.value.push(r));
  currentScreen.value = 'started';
}
</script>

<template>
  <ConfigTrivia @selected-config="requestQuestions" v-if="currentScreen === 'config'" />
  <TriviaGame :results="results" v-if="currentScreen === 'started'"/>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
