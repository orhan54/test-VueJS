<template>
  <div class="container">
    <div v-if="state === 'error'">
      <p>Impossible de charger le quiz</p>
    </div>

    <div v-else-if="state === 'loading'" aria-busy="true">
      <p>Chargement...</p>
    </div>

    <div v-else>
      <Quiz v-if="quiz" :quiz="quiz" />
      <p v-else>Quiz introuvable</p>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import Quiz from './components/Quiz.vue';

const quiz = ref(null)
const state = ref('loading')

onMounted(() => {
  fetch('/quiz.json')
    .then(r => {
      if (!r.ok) throw new Error('Impossible de récupérer le JSON');
      return r.json()
    })
    .then(data => {
      // Récupérer le quiz par slug (depuis URL)
      const params = new URLSearchParams(window.location.search)
      const slug = params.get('slug') || 'films-series'
      quiz.value = data.find(q => q.slug === slug)
      state.value = 'idle'
    })
    .catch(() => {
      state.value = 'error'
    })
})
</script>

<style>
.container {
  margin-top: 2rem;
}
</style>