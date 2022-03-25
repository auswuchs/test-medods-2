<script setup>
import { ref, } from 'vue';

const sequence = ref([])
const sequenceToGuess = ref([])

const canClick = ref(false)
const canStart = ref(true)
const isGameOver = ref(false)

const flashDuration = ref(300)
const flashInterval = ref(1500)
const currentMove = ref(0)


/**
 * Play sound and change style on clicked panel.
 * @param {event} e The element click event.
 */
const mouseDown = (e) => {
  if (!canClick.value) return

  e.target.style.opacity = 1

  const pressedPanel = +e.target.dataset.panel
  playSound(pressedPanel)
}

/**
 * Сhange style on clicked panel to default.
 * @param {event} e The element click event.
 */
const mouseUp = (e) => {
  if (!canClick.value) return

  e.target.style.opacity = null

  const pressedPanel = +e.target.dataset.panel
  validatePanel(pressedPanel)
}

/**
 * Set styles on flashing panel.
 * @param {number} panel The data-panel number from 1 to 4.
 */
const setStyles = (panel) => {
  const el = document.querySelector(`[data-panel='${panel}']`)
  el.style.opacity = 1
}

/**
 * Remove styles on flashing panel.
 * @param {number} panel The data-panel number from 1 to 4.
 */
const removeStyles = (panel) => {
  const el = document.querySelector(`[data-panel='${panel}']`)
  el.style.opacity = null
}

/**
 * Set values to their defaults.
 */
const gameover = () => {
  sequence.value = []
  canStart.value = true
}

/**
 * Validate if clicked the right one panel.
 * @param {number} pressedPanel The data-panel number from 1 to 4.
 */
const validatePanel = (pressedPanel) => {
  const expectedPanel = sequenceToGuess.value.shift()
  const res = pressedPanel === expectedPanel

  if (res) {
    if (sequenceToGuess.value.length === 0) {
      canStart.value = true
      startGame()
    }
  } else {
    isGameOver.value = true
    gameover()
  }
}

/**
 * Push random number from 1 to 4 to an sequence array. 
 * Copy the values of the sequence array to sueqenceToGuess.
 */
const getRandomPanel = () => {
  sequence.value.push(Math.floor(Math.random() * 4) + 1)
  sequenceToGuess.value = [...sequence.value]
}

/**
 * Play sound of current panel.
 * @param {number} panelNumber The data-panel number from 1 to 4.
 */
const playSound = (panelNumber) => {
  const audio = `/sounds/${panelNumber}.mp3`
  const note = new Audio(audio);
  note.play()
}

/**
 * Play sound of clicked panel. Set panel styles
 * after flashInterval for flashDuration time.
 * @param {number} panel The data-panel number from 1 to 4.
 * @return {promise} The result of function completion.
 */
const flash = (panel) => {


  return new Promise((res) => {
    setTimeout(() => {
      setStyles(panel)
      playSound(panel)

      setTimeout(() => {
        removeStyles(panel)
        res()
      }, flashDuration.value)

    }, flashInterval.value)
  })
}

/**
 * Loop trough the sequence array, to flash
 * every panel in it.
 */
const loop = async () => {
  for (const panel of sequence.value) {
    await flash(panel)
  }
}

/**
 * Checks if can start a game. 
 * Checks if the game over yet. If so, sets current 
 * round and gameover to default values to start a new game. 
 * Prevent random panels click and start the game while game started.
 * After completion allowa to click panels.
 */
const startGame = async () => {
  if (!canStart.value) return
  if (isGameOver.value) {
    currentMove.value = 0
    isGameOver.value = false
  }

  canClick.value = false
  canStart.value = false
  currentMove.value += 1

  getRandomPanel()
  await loop()

  canClick.value = true
}

</script>

<template>
  <div class="wrapper">
    <button class="panel t-l-panel" @mousedown="mouseDown" @mouseup="mouseUp" data-panel="1"></button>
    <button class="panel t-r-panel" @mousedown="mouseDown" @mouseup="mouseUp" data-panel="2"></button>
    <button class="panel b-l-panel" @mousedown="mouseDown" @mouseup="mouseUp" data-panel="3"></button>
    <button class="panel b-r-panel" @mousedown="mouseDown" @mouseup="mouseUp" data-panel="4"></button>
  </div>
  <div class="controls">
    <h2>Round: {{ currentMove }}</h2>
    <button class="controls-start" @click="startGame">Start</button>
    <p class="controls-gameover" v-if="isGameOver">Sorry, you lost on round {{ currentMove }}</p>

    <h2>Difficulty</h2>
    <div class="controls-options">
      <input type="radio" name="difficulty" id="easy" v-model="flashInterval" value="1500" checked />
      <label for="easy">Easy</label>

      <input type="radio" name="difficulty" id="medium" v-model="flashInterval" value="1000" />
      <label for="medium">Medium</label>

      <input type="radio" name="difficulty" id="hard" v-model="flashInterval" value="400" />
      <label for="hard">Hard</label>
    </div>
  </div>
</template>

<style scoped lang="scss">
/* Wrapper */
.wrapper {
  display: grid;
  max-width: 250px;
  max-height: 250px;
  grid-template-columns: repeat(2, 125px);
  grid-template-rows: repeat(2, 125px);
  background-color: white;
  box-shadow: 0 0 7px 3px #aaa;
  border-radius: 100%;
  z-index: 10;
}

.panel {
  justify-self: center;
  width: 125px;
  height: 125px;
  border: none;
  opacity: 0.5;
}

.t-l-panel {
  background: dodgerblue;
  border-top-left-radius: 100%;
  border-top: 3px solid transparent;
  border-left: 3px solid transparent;

  &:hover {
    border-top-color: white;
    border-left-color: white;
  }
}

.t-r-panel {
  background: #ff5643;
  border-top-right-radius: 100%;

  border-top: 3px solid transparent;
  border-right: 3px solid transparent;

  &:hover {
    border-top-color: white;
    border-right-color: white;
  }
}

.b-l-panel {
  background: #feef33;
  border-bottom-left-radius: 100%;

  border-bottom: 3px solid transparent;
  border-left: 3px solid transparent;

  &:hover {
    border-bottom-color: white;
    border-left-color: white;
  }
}

.b-r-panel {
  background: #bede15;
  border-bottom-right-radius: 100%;

  border-bottom: 3px solid transparent;
  border-right: 3px solid transparent;

  &:hover {
    border-bottom-color: white;
    border-right-color: white;
  }
}

/* Controls */
.controls {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.controls-start {
  max-width: max-content;
  padding: 10px 25px;
  border: none;
  background-color: teal;
  color: aliceblue;
  cursor: pointer;
  margin: 1rem 0;
}
.controls-gameover {
  margin-bottom: 1rem;
}
.controls-options {
  display: flex;
  flex-wrap: wrap;
  align-items: center;

  & input {
    flex: 0 1 20%;
  }

  & label {
    flex: 1 0 80%;
  }
}
</style>
