<template>
  <div class="game">
    <h1 style="color: orangered;">Tic Tac Toe</h1>
    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        @click="makeMove(index)"
        class="cell"
      >
        <img v-if="cell" :src="getImageSource(cell)" :alt="cell" />
      </div>
    </div>
    <p style="display: inline;" v-if="winner">Winner: </p> <img class="img" v-if="winner" :src="getImageSource(winner)" :alt="winner" />
    <p v-else-if="isDraw">It's a draw!</p>
    <br/>
    <button @click="resetGame">Reset Game</button>
    <Confetti v-if="winner" />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import Confetti from './Confetti.vue';

import vueIcon from '../assets/vue.svg';
import viteIcon from '../assets/vite.svg';

const board = ref<string[]>(['', '', '', '', '', '', '', '', '']);
const currentPlayer = ref<'X' | 'O'>('X');
const winner = ref<'X' | 'O' | null>(null);
const isDraw = ref(false);
const winSound = ref<HTMLAudioElement | null>(null);
const clapSound = ref<HTMLAudioElement | null>(null);

const getImageSource = (cell: string) => {
  return cell === 'X' ? vueIcon : viteIcon;
};

const makeMove = (index: number) => {
  if (board.value[index] || winner.value || isDraw.value) return;
  board.value[index] = currentPlayer.value;
  board.value = [...board.value]; // Trigger reactivity
  checkWinner();
  currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
};

const checkWinner = () => {
  const winConditions = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (const condition of winConditions) {
    const [a, b, c] = condition;
    if (
      board.value[a] &&
      board.value[a] === board.value[b] &&
      board.value[a] === board.value[c]
    ) {
      winner.value = board.value[a] as 'X' | 'O';
      winSound.value?.play();
      clapSound.value?.play();
      return;
    }
  }
  if (!board.value.includes('')) {
    isDraw.value = true;
  }
};

const resetGame = () => {
  board.value = ['', '', '', '', '', '', '', '', ''];
  currentPlayer.value = 'X';
  winner.value = null;
  isDraw.value = false;
};

onMounted(() => {
  winSound.value = new Audio('https://www.freesound.org/data/previews/341/341891_4664166-lq.mp3');
  clapSound.value = new Audio('https://www.freesound.org/data/previews/220/220589_253703-lq.mp3');
});
</script>

<style scoped>
.game {
  text-align: center;
}
.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  max-width: 300px;
  margin: 0 auto;
}
.cell {
  aspect-ratio: 1 / 1;
  border: 2px solid #ccc;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
}
.cell img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}
.img {
  max-width: 10%;
  max-height: 10%;
  object-fit: scale-down;
  /* display: inline; */
}
</style>