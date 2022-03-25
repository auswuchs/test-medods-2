<script setup>
</script>

<template>
  <div class="wrapper">
    <button class="panel t-l-panel"></button>
    <button class="panel t-r--panel"></button>
    <button class="panel b-l-panel"></button>
    <button class="panel b-r-panel"></button>
  </div>
</template>

<style scoped lang="scss">
.wrapper {
  display: grid;
  max-width: 250px;
  max-height: 250px;
  grid-template-columns: repeat(2, 100px);
  grid-template-rows: repeat(2, 100px);
  background-color: aliceblue;
  border-radius: 50%;
}

.panel {
  justify-self: center;
  width: 100px;
  height: 100px;
  border: none;
  background-color: aqua;
}

.t-l-panel {
  background: dodgerblue;
  opacity: 0.6;
}
</style>
