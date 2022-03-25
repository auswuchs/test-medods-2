<script setup>
import TheGame from './components/TheGame.vue'
</script>

<template>
  <main class="main">
    <The-Game />
  </main>
</template>

<style lang="scss">
#app {
  max-width: 1280px;
  min-height: 100vh;
  max-height: 100vh;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

.main {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 75px;
  position: relative;
  height: 70vh;
}
</style>
