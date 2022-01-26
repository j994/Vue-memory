<script setup>
import GameBoard from './components/GameBoard.vue';
</script>

<template>
  <GameBoard />
</template>


<style lang="scss">
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  scroll-behavior: smooth;
  scrollbar-color: transparent transparent;
  scrollbar-width: none;
  &::-webkit-scrollbar {
    width: 0px;
    background: transparent;
  }
}
</style>
