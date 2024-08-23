<template>
    <div class="game">
      <h1 style="color: orangered;">Tic Tac Toe</h1>
      <div class="board">
        <div 
          v-for="(cell, index) in board" 
          :key="index" 
          @click="makeMove(index)"
        >
          {{ cell }}
        </div>
      </div>
      <p v-if="winner">Winner: {{ winner }}</p>
      <p v-else-if="isDraw">It's a draw!</p>
      <br/>
      <button @click="resetGame">Reset Game</button>
      <Confetti v-if="winner" />
      <!-- <audio v-if="winner" ref="winSound" src="https://cdn.freesound.org/previews/320/320672_5260872-lq.mp3" autoplay loop></audio> -->
      <!-- <audio v-if="winner" ref="clapSound" src="https://cdn.freesound.org/previews/612/612269_13578264-lq.mp3" autoplay></audio> -->
    </div>
  </template>
  

  
  <script setup lang="ts">
  import { ref, onMounted } from 'vue';
  import Confetti from './Confetti.vue';
  
  const board = ref<string[]>(['', '', '', '', '', '', '', '', '']);
  const currentPlayer = ref<'X' | 'O'>('X');
  const winner = ref<'X' | 'O' | null>(null);
  const isDraw = ref(false);
  const winSound = ref<HTMLAudioElement | null>(null);
  const clapSound = ref<HTMLAudioElement | null>(null);
  
  const makeMove = (index: number) => {
    if (board.value[index] || winner.value || isDraw.value) return;
    board.value[index] = currentPlayer.value;
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
    
    click.active {
      background-color: #cccc;
      color: #fff;
      font-size: xx-large;  
    }
    text-align: center;
    /* text-size-adjust: 100%;
    font-size: 16x;
    line-height: 1.5;
    font-weight: 99;
    font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif; */
    /* color: #2c3e50; */
  }
  .board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
  }
  .board div {
    padding: 60px;
    border: 8px solid #ccc;
    cursor: pointer;
  }
  </style>

