<script setup lang="ts">
import { ref, computed } from 'vue';
import { Username } from '../models/Username';
// import { resetGame } from '../models/resetGame';

const board = ref(Array(9).fill(''));
let currentPlayer = ref('X');
let gameOver = ref(false);
let winner = ref(null);

const makeMove = (index: number): void => {
  if (board.value[index] === '' && !gameOver.value) {
    board.value[index] = currentPlayer.value;
    checkWinner();
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
  }
};

const checkWinner = (): void => {
  const winningCombinations = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (const combination of winningCombinations) {
    const [a, b, c] = combination;
    if (
      board.value[a] !== '' &&
      board.value[a] === board.value[b] &&
      board.value[a] === board.value[c]
    ) {
      gameOver.value = true;
      winner.value = board.value[a];
      break;
    }
  }
};

const resetGame = (): void => {
  board.value = Array(9).fill('');
  currentPlayer.value = winner.value === 'X' ? 'O' : 'X';
  gameOver.value = false;
  winner.value = null;
};

const gameStatus = computed(() => {
  if (gameOver.value) {
    if (winner.value === 'X') {
      return `${Username.names[0]} wins!`;
    } else if (winner.value === 'O') {
      return `${Username.names[1]} wins!`;
    }
  } else if (board.value.every(cell => cell !== '')) {
    return "Tie!";
  } else {
    const currentPlayerIndex = currentPlayer.value === 'X' ? 0 : 1;
    return `${Username.names[currentPlayerIndex]}'s turn`;
  }
});




</script>

<template>
  <p>{{ gameStatus }}</p>
  <div class="gameboard">
    <div class="cell" v-for="(cell, index) in board" :key="index" @click="makeMove(index)">
      <span :class="cell">{{ cell }}</span>
    </div>
  </div>

  <button @click="resetGame">Reset</button>
</template>

<style scoped>
.gameboard {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
  width: 300px;
  margin-bottom: 20px;
}

.cell {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #25d8b7;
  cursor: pointer;
  font-size: 24px;
  height: 100px;
}

.X {
  color: white;
}

.O {
  color: black;
}
</style>
