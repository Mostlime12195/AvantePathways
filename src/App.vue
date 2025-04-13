<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import QuizComponent from './components/QuizComponent.vue'
</script>

<template>
  <main>
    <QuizComponent />
  </main>
</template>

<style>
/* Reset ALL default Vue styles */
#app {
  width: 100vw;
  max-width: none;
  margin: 0;
  padding: 0;
  display: block;
  /* Override Vue's default grid */
}

:root {
  --primary-gradient: linear-gradient(135deg, #ff00ff, #8a2be2);
  --glow-color: rgba(255, 0, 255, 0.5);
  --neon-purple: #b026ff;
  color-scheme: dark;
}

body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
  width: 100vw;
  background: #0a0a0a;
  color: white;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  overflow-x: hidden;
}

main {
  min-height: 100vh;
  width: 100vw;
  margin: 0;
  padding: 0;
  background: linear-gradient(135deg, #1a0033, #000000);
  display: block;
}

@media (min-width: 1024px) {
  #app {
    display: block;
    grid-template-columns: none;
    padding: 0;
  }
}

@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;600;700&family=Rajdhani:wght@400;500;600;700&display=swap');
</style>
