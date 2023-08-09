<script setup>
import { ref } from "vue";
import ConfigTrivia from "./components/Form/ConfigTrivia.vue";
import TriviaGame from "./components/Form/TriviaGame.vue";
import MessageTerminate from "./components/Form/MessageTerminate.vue";

const results = ref([]);
const currentScreen = ref("config");
const score = ref(0);

async function requestQuestions({ category, difficulty }) {
  let url =
    "https://opentdb.com/api.php?amount=5&category=:category&difficulty=:difficulty&type=multiple";
  url = url.replace(":category", category).replace(":difficulty", difficulty);
  const response = await fetch(url);
  const data = await response.json();
  data.results.forEach(r => results.value.push(r));
  currentScreen.value = 'started';
}
function onTerminatedTrivia() {
  currentScreen.value = 'terminated';
}
function onRestart() {
  currentScreen.value = 'config';
  score.value = 0;
  results.value = [];
}
</script>

<template>
  <div class="points-container">Points: {{ score }}</div>
  <ConfigTrivia @selected-config="requestQuestions" v-if="currentScreen === 'config'" />
  <TriviaGame v-model="score" :results="results" @terminated-trivia="onTerminatedTrivia" v-if="currentScreen === 'started'"/>
  <MessageTerminate :score="score" @restart="onRestart" v-if="currentScreen === 'terminated'"/>
</template>
<style>
.points-container {
  position: absolute;
  right: 12px;
  top: 12px;
}
</style>
