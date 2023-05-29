<template>
  <div class="screen">
    <div class="board">
      <div
        class="tile"
        v-for="tile in tiles"
        :key="tile.x + tile.y"
        :style="tileStyle(tile)"
        @click="handleTileClick(tile)"
      >
        {{ tile.x }}, {{ tile.y }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed, watchEffect } from "vue";

// create a 4x4 grid of tiles
const tiles = ref([]);
const shuffledColors = computed(() => {
  return colors.sort(() => Math.random() - 0.5);
});
const gridSize = 3;
const lookForColors = ref([]);

const colors = [
  "red",
  "blue",
  "green",
  "yellow",
  "orange",
  "purple",
  "pink",
  "cyan",
  "brown",
  "black",
  "white",
  "gray",
  "magenta",
  "lime",
  "olive",
  "maroon",
  "navy",
  "teal",
  "aqua",
];

onMounted(() => {
  // game starts
  startNewGame();
});

const startNewGame = async () => {
  // set the tiles on the board, in order
  tiles.value = generateGrid(gridSize * gridSize);

  // wait 2 seconds
  await new Promise((resolve) => setTimeout(resolve, 2000));

  // get an array of all colors of the tiles
  lookForColors.value = tiles.value.map((tile) => tile.x);
};

const handleTileClick = (tile) => {
  if (randomTiles.value.includes(tile)) {
    tile.color = "green";
  } else {
    startNewGame();
  }
};

const generateGrid = (nOfTiles) => {
  const grid = [];
  for (let i = 0; i < nOfTiles; i++) {
    grid.push({
      x: i % gridSize,
      y: Math.floor(i / gridSize),
      color: colors[Math.floor(Math.random() * colors.length)],
    });
  }
  return grid;
};

// computed styles for each tile
const tileStyle = (tile) => ({
  width: `${100 / gridSize}%`,
  height: `${100 / gridSize}%`,
  backgroundColor: tile.color,
});
</script>

<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
.screen {
  position: fixed;
  width: 100%;
  height: 100vh;
  top: 0;
  left: 0;
  background-color: #7c7979;
  overflow: hidden;
}
.board {
  width: 20vw;
  height: 20vw;
  border: 5px solid black;
  position: relative;
  display: flex;
  flex-wrap: wrap;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.tile {
  border: 1px solid black;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
