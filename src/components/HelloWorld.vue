<template>
  <div id="app">
    <div class="game-board">
      <div class="x-ray"></div>
      <div class="player-aura">
        <div class="player" ref="player"></div>
      </div>
      <div class="enemy" ref="enemy" :style="enemyStyles"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed, watchEffect } from "vue";

let enemyInterval = null;

const enemy = ref(null);
const gameSpeed = ref(200);
const enemyPosition = ref(0);
const currentArrow = ref(null);
const hit = ref(false);

const arrows = [
  {
    code: "&#8592;",
    keyCode: "ArrowLeft",
    clipPath: "polygon(0 50%, 100% 0, 100% 100%, 0 50%)",
    color: "orange",
    defeated: 0,
  },
  {
    code: "&#8593;",
    keyCode: "ArrowUp",
    clipPath: "polygon(50% 0, 50% 0, 100% 100%, 0% 100%)",
    color: "cyan",
    defeated: 0,
  },
  {
    code: "&#8594;",
    keyCode: "ArrowRight",
    clipPath: "polygon(0 0, 100% 50%, 100% 50%, 0 100%)",
    color: "pink",
    defeated: 0,
  },
  {
    code: "&#8595;",
    keyCode: "ArrowDown",
    clipPath: "polygon(0 0, 100% 0, 50% 100%, 50% 100%)",
    color: "yellow",
    defeated: 0,
  },
]; // same arrow data

// Random arrow selector
const selectRandomArrow = () =>
  arrows[Math.floor(Math.random() * arrows.length)];

// Computed styles for enemy
const enemyStyles = computed(() => ({
  right: `${enemyPosition.value}%`,
  backgroundColor:
    enemyPosition.value >= 80 ? "white" : currentArrow.value.color,
  clipPath:
    arrows.find((arrow) => arrow.keyCode === currentArrow.value.keyCode)
      .defeated === 0
      ? currentArrow.value.clipPath
      : "none",
}));

// Cleanup function for game reset
const cleanupGame = () => {
  if (enemyInterval) {
    clearInterval(enemyInterval);
  }
  enemyPosition.value = 0;
};

// New game function
const newGame = () => {
  cleanupGame();
  currentArrow.value = selectRandomArrow();
  startGame();
};

// Game logic function
const startGame = () => {
  enemyInterval = setInterval(() => {
    enemyPosition.value += 5;
    if (enemyPosition.value >= 100) {
      enemyPosition.value = 0;
      console.log("game over");
    }
  }, gameSpeed.value);
};

// Keydown event handler
const handleKeydown = (e) => {
  if (e.code === currentArrow.value.keyCode && enemyPosition.value > 80) {
    hit.value = true;
  }
};

// Watch for hit and start new game
watchEffect(() => {
  if (hit.value) {
    // add to the defeated of arrow
    currentArrow.value.defeated += 1;
    console.log(currentArrow.value.defeated);
    hit.value = false;
    newGame();
  }
});

// Setup and cleanup keydown event listener
onMounted(() => {
  window.addEventListener("keydown", handleKeydown);
});
onBeforeUnmount(() => {
  window.removeEventListener("keydown", handleKeydown);
});

// Initialize the game
newGame();
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
