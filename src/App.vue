<script lang="ts">
import TicTacToe from './components/TicTacToe.vue'
const str = "Hello World"
export default {
  name: "App",
  components: {
    TicTacToe
  },
  data() {
    return {
      str: "This is my Tictactoe Game!"
    }
  }
}
</script>

<template>
  <TicTacToe :str="str" />
</template>