<template>
  <p v-if="winner==='Tie'">It's a {{ winner }}</p>
  <p v-if="!!winner && winner!=='Tie'">Congratulations, Player {{ winner }} wins!</p>
  <div class="wrapper">
    <button v-for="(cell, index) in cells" :key="index" @click="handleClick(index)" :class="{'player-1': cell === 'X', 'player-2': cell === 'O'}">{{ cell }}</button>
  </div>
  <button @click="resetGame">Reset</button>
</template>

<script lang="ts">
import { defineComponent, reactive, Ref, watch } from 'vue';

interface TicTacToeState {
  cells: string[];
  currentPlayer: string;
  winner: string | null;
}

export default defineComponent({
  setup() {
    const state: TicTacToeState = reactive({
      cells: Array(9).fill(''),
      currentPlayer: 'X',
      winner: null
    });

    const handleClick = (index: number): void => {
      if (!state.cells[index] && !state.winner) {
        state.cells[index] = state.currentPlayer;
        checkWinner();
        state.currentPlayer = state.currentPlayer === 'X' ? 'O' : 'X';
      }
    };

    const checkWinner = (): void => {
      const winningCombos = ['012', '345', '678', '036', '147', '258', '048', '246'];
      for (const combo of winningCombos) {
        const [a, b, c] = combo.split('');
        if (state.cells[+a] && state.cells[+a] === state.cells[+b] && state.cells[+b] === state.cells[+c]) {
          state.winner = state.currentPlayer;
          return;
        }
      }
      if (!state.cells.includes('')) {
        state.winner = 'Tie';
      }
    };

    const resetGame = (): void => {
      state.cells.fill('');
      state.currentPlayer = 'X';
      state.winner = null;
    };

    watch(
      () => state.winner,
      (newValue: string | null) => {
        if (newValue) {
          console.log(`Congratulations, Player ${newValue} wins!`);
        }
      }
    );

    return {
      cells: state.cells,
      winner: state.winner,
      handleClick,
      resetGame
    };
  }
});
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
.player-1 {
    color: blue;
  }
.player-2 {
  color: red;
}
</style>