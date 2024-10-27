<template>
  <div class="container">
    <div v-if="state === 'error'">
      <p>Impossible de charger le quiz</p>
    </div>
    <div :aria-busy="state === 'loading'">
      <Quiz :quiz="quiz" v-if="quiz" />
    </div>
  </div>
</template>

<script setup>
import Quiz from "./composents/Quiz.vue";
import { onMounted, ref } from "vue";
const quiz = ref(null);
const state = ref("loading");

onMounted(() => {
  fetch("/quiz.json")
    .then((response) => {
      if (!response.ok) {
        throw new Error("impossible de récupérer le json");
      }
      return response.json();
    })
    .then((data) => {
      quiz.value = data;
      state.value = "idle";
    })
    .catch(() => {
      state.value = "error";
    });
});
</script>

<style>
.container {
  margin-top: 2rem;
}
</style>
