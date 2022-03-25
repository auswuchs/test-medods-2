<script setup>
import TheWelcome from './components/TheWelcome.vue'
</script>

<template>
  <main class="main">
    <TheWelcome />
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
  display: grid;
  justify-items: center;
  align-items: center;
  height: 70vh;
}
</style>
