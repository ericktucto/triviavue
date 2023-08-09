<script setup>
import { ref, onMounted, defineProps } from "vue";
const props = defineProps({
  score: 0,
});
function compartir() {
  const url = "https://triviavue.vercel.app/";
  const data = {
    title: 'Puntaje en mi trivia',
    text: `Obtuve ${props.score} punto(s) en mi trivia de https://triviavue.vercel.app/!`,
    files: []
  }
  navigator.share(data)
}
const vercompartir = ref(false);
onMounted(() => {
  const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
  const isSecure = location.protocol === "https:";
  if (isMobile && isSecure) {
    vercompartir.value = true;
  }
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
      <button type="button" v-if="vercompartir" class="btn-outline" @click="compartir">Share</button>
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
