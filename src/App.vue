<template>
  <!-- <HelloWorld /> -->
  <!-- <MemoryMatrix /> -->
  <MemoryMatrix2 />
  <DotToDot />/
</template>

<script setup>
// import DotToDot from "./components/DotToDot.vue";
import MemoryMatrix2 from "./components/MemoryMatrix2.vue";
</script>

<style scoped>
#app {
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  padding: 0;
  font-size: 1rem;
}

.game-board {
  transform: translate(5%, 5%);
  width: 90%;
  height: 50%;
  position: relative;
}

.player,
.enemy {
  width: 20px;
  height: 20px;
  position: absolute;
  transition: all 0.2s linear;
}

.player-aura {
  left: 0;
  bottom: 0;
  position: absolute;
  width: 15%;
  height: 100px;
  border-right: 10px solid rgba(194, 245, 112, 0.1);
  transform: translateX(10%);
}
.player {
  left: 0;
  bottom: 0;
  background: green;
  line-height: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.enemy {
  right: 0;
  bottom: 0;
  background: red;
  line-height: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.x-ray {
  width: 100%;
  height: 100%;
  position: absolute;
  right: 0;
  top: 0;
  width: 50%;
  background-color: rgba(0, 0, 0, 0.5);
}
</style>
