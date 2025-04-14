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
  --secondary-gradient: linear-gradient(45deg, #6a0dad, #ff1493);
  --glow-color: rgba(255, 0, 255, 0.5);
  --neon-purple: #b026ff;
  --neon-pink: #ff1493;
  --bg-dark: rgba(18, 18, 18, 0.95);
  --bg-darker: rgba(0, 0, 0, 0.9);
  color-scheme: dark;
}

body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
  width: 100vw;
  background: #0a0a0a;
  color: #E0E0E0;
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
  position: relative;
}

main::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: radial-gradient(circle at 50% 50%, rgba(138, 43, 226, 0.15), transparent 70%);
  pointer-events: none;
}

@keyframes float {
  0% {
    transform: translateY(0px);
  }

  50% {
    transform: translateY(-10px);
  }

  100% {
    transform: translateY(0px);
  }
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: 1;
  }

  50% {
    transform: scale(1.05);
    opacity: 0.8;
  }

  100% {
    transform: scale(1);
    opacity: 1;
  }
}

@keyframes glow {
  0% {
    box-shadow: 0 0 5px var(--glow-color);
  }

  50% {
    box-shadow: 0 0 20px var(--glow-color), 0 0 40px var(--neon-purple);
  }

  100% {
    box-shadow: 0 0 5px var(--glow-color);
  }
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
