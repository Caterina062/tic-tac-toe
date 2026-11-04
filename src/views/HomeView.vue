<script setup>
import { ref, computed } from 'vue'

const currentPlayer = ref('X')
const board = ref(Array(9).fill('')) // initialize the board with empty strings

const vittorieX = ref(0)
const vittorieO = ref(0)

const winningCombinations = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
]

const winner = computed(() => {
  for (const combination of winningCombinations) {
    const [a, b, c] = combination

    if (board.value[a] && board.value[a] === board.value[b] && board.value[a] === board.value[c]) {
      return board.value[a]
    }
  }
  return null
})

const isDraw = computed(() => {
  return !winner.value && board.value.every((cell) => cell !== '')
})

function play(index) {
  // Check if the cell is already occupied
  if (board.value[index] !== '' || winner.value || isDraw.value) return

  board.value[index] = currentPlayer.value

  if (winner.value === 'X') {
    vittorieX.value++
  } else if (winner.value === 'O') {
    vittorieO.value++
  } else {
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'
  }
}

function resetGame() {
  board.value = Array(9).fill('')
  currentPlayer.value = 'X'
}
</script>

<template>
  <main class="container">
    <h1 class="title">Tic Tac Toe</h1>

    <div class="vittorie">
      <div>
        <p class="vincite">Giocatore X:</p>
        <p class="vincite">{{ vittorieX }}</p>
      </div>
      <div>
        <p class="vincite">Giocatore O:</p>
        <p class="vincite">{{ vittorieO }}</p>
      </div>
    </div>

    <div class="status">
      <p v-if="winner" class="message winner">Vincitore: {{ winner }} !</p>
      <p v-else-if="isDraw" class="message draw">Pareggio!</p>
      <p v-else class="current-player">È il turno del giocatore: {{ currentPlayer }}</p>
    </div>

    <div class="game-grid">
      <button
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        :class="{ 'is-x': cell === 'X', 'is-o': cell === 'O' }"
        :disabled="cell !== '' || winner || isDraw"
        @click="play(index)"
      >
        {{ cell }}
      </button>
    </div>

    <button class="reset-btn" @click="resetGame">Nuova Partita</button>
  </main>
</template>

<style setup>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  background-color: #b6c4cc;
  font-family: Arial, sans-serif;
}

.title {
  font-size: 2.5rem;
  text-align: center;
  margin-top: 2rem;
}

.current-player .message {
  font-size: 1.5rem;
  margin-bottom: 1.5rem;
  padding: 0.5rem;
}

.status {
  margin-bottom: 1.5rem;
}
.winner {
  color: #2f9936;
  font-weight: bold;
  font-size: 1.5rem;
}

.draw {
  color: #821e1e;
  font-weight: bold;
  font-size: 1.5rem;
}

.game-grid {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 8px;
}

.cell {
  width: 100px;
  height: 100px;
  font-size: 2.5rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.1s ease-in;
  margin: 5px;
  background-color: #ecf0f2;
  border-radius: 7px;
  border: none;
}

.cell.is-x {
  color: rgb(192, 72, 35);
}
.cell.is-o {
  color: rgb(44, 126, 213);
}

.cell:hover:not(:disabled) {
  background-color: #a4aeb6;
}

.reset-btn {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  font-size: 1.2rem;
  cursor: pointer;
  background-color: #1e88e5;
  color: white;
  border: none;
  border-radius: 6px;
}
.vittorie {
  display: flex;
  flex-direction: row;
}
.vittorie > div {
  margin: 0 20px;
  text-align: center;
  font-size: 1.2rem;
  font-weight: bold;
}

.vincite {
  padding: 0 0 10px 0;
  margin: 0;
}
</style>
