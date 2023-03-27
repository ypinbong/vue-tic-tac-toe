<template>
  <div id="app">
    <p v-if="winner === 'Tie'">It's a {{ winner }}</p>
    <p v-if="!!winner && winner !== 'Tie'">Congratulations, Player {{ winner }} wins!</p>
    <div class="wrapper">
      <button v-for="(cell, index) in cells" :key="index" @click="handleClick(index)" :class="{'player-1': cell === 'X', 'player-2': cell === 'O'}">{{ cell }}</button>
    </div>
    <button @click="resetGame">Reset</button>
  </div>
</template>

<script lang="ts">
  export default {
    data(){
      return{
        cells: Array(9).fill(''),
        currentPlayer: 'X',
        winner: null || '',
      }
    },
    methods: {
      handleClick(index: number) {
        if (!this.cells[index] && !this.winner) {
          this.cells[index] = this.currentPlayer;
          this.checkWinner();
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
        }
      },
      checkWinner() {
        const winningCombos = ['012', '345', '678', '036', '147', '258', '048', '246'];
        for (const combo of winningCombos) {
          const [a, b, c] = combo.split('');
          if (this.cells[a] && this.cells[a] === this.cells[b] && this.cells[b] === this.cells[c]) {
            this.winner = this.currentPlayer;
            return;
          }
        }
        if (!this.cells.includes('')) {
          this.winner = 'Tie';
        }
      },
      resetGame() {
        this.cells.fill('');
        this.currentPlayer = 'X';
        this.winner = null || '';
      },
    },
  }
</script>

<style scoped>
.wrapper{
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
button{
  color: white;
  width: 80px;
  height: 80px;
}
</style>