<template>
  <div style="margin-bottom: 10px;" v-if="winner">The winner is {{ winner }}</div>
  <div class="wrapper" >
    <div v-for="index in 9" :key="index">
      <button @click="handleClick(index)" :disabled="!!winner">{{ ticArr[index-1]?.state }}</button>
    </div>
  </div>
  <button style="margin-top: 10px" @click="resetBoard">Reset</button>
</template>

<script lang="ts">
  export default {
    data(){
      return{
        count: 0,
        ticArr: [{
          index: 0,
          state: ""
        }],
        ticType: "X",
        winner: "",
        disableTic: false,
      }
    },
    computed: {
      validTic() {
        return this.ticArr.filter(tic => tic.state)
      }
    },
    methods: {
      handleClick(index:number){
        this.ticArr[index-1] = {index: index, state: this.ticType};
        this.ticType = this.ticType === "X" ? "O" : "X";
        this.checkBoard()
      },
      checkBoard(){
        if (this.validTic.length < 5) return;
        const xMoves = this.validTic.filter(item => item.state === "X").map(item => item.index).sort((a, b) => a-b).join("");
        const oMoves = this.validTic.filter(item => item.state === "O").map(item => item.index).sort((a, b) => a-b).join("");
        
        let hasWinner;
        if (xMoves.length >= 3) {
          hasWinner = this.hasWinningCombo(xMoves)
          if (hasWinner) {
            this.winner = "X";
          }
        }
        if (oMoves.length >= 3) {
          hasWinner = this.hasWinningCombo(oMoves)
          if (hasWinner) {
            this.winner = "O";
          }
        }
      },
      hasWinningCombo(str: string): boolean{
        const winningCombos = ['123', '456', '789', '147', '258', '369', '159', '357'];
        const res = winningCombos.some(combo => str.includes(combo));
        console.log("TicTacToe.vue ~ line 56: res", res);
        return res;
      },
      resetBoard(){
        this.ticArr = [];
        this.ticType = "X";
        this.winner = "";
      },
    }
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