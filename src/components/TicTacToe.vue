<template>
  <p v-if="winner==='Tie'">It's a {{ winner }}</p>
  <p v-if="!!winner && winner!=='Tie'">Congratulations, Player {{ winner }} wins!</p>
  <div class="wrapper">
    <button v-for="(cell, index) in cells" :key="index" @click="handleClick(index)" :class="{'player-1': cell === 'X', 'player-2': cell === 'O'}">{{ cell }}</button>
  </div>
  <button @click="resetGame">Reset</button>
</template>

<script lang="ts" setup>
import { ref } from 'vue';

const cells = ref<string[]>(Array(9).fill(''));
const currentPlayer = ref<string>('X');
const winner = ref<string | null>(null);

function handleClick(index: number) {
  if (!cells.value[index] && !winner.value) {
    cells.value[index] = currentPlayer.value;
    checkWinner();
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
  }
}

function checkWinner() {
  const winningCombos = ['012', '345', '678', '036', '147', '258', '048', '246'];
  for (const combo of winningCombos) {
    const [a, b, c] = combo.split('');
    if (cells.value[+a] && cells.value[+a] === cells.value[+b] && cells.value[+b] === cells.value[+c]) {
      winner.value = currentPlayer.value;
      return;
    }
  }
  if (!cells.value.includes('')) {
    winner.value = 'Tie';
  }
}

function resetGame() {
  cells.value.fill('');
  currentPlayer.value = 'X';
  winner.value = null;
}
</script>

<style scoped>
.wrapper{
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
button{
  width: 80px;
  height: 80px;
}
</style>