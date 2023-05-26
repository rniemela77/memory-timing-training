<template>
  <div id="app" v-if="isLoading">
    <div class="game-board">
      <div class="x-ray"></div>

      <div class="player-aura">
        <div class="player" ref="player"></div>
      </div>
      <div class="enemy" ref="enemy" :style="currClipPath"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed, watch } from "vue";

const isLoading = ref(true);

const currClipPath = computed(() => {
  return enemyPosition.value < 50 && currentArrow.value.defeated < 1
    ? { clipPath: currentArrow.value.clipPath }
    : { clipPath: "polygon(0 0, 100% 0, 100% 100%, 0% 100%)" };
});

const gameSpeed = ref(200);
const enemySpeed = ref(1);
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
];
const currentArrow = ref(null);
const enemyPosition = ref(0);

let enemyInterval = null;

const enemy = ref(null);

const newGame = () => {
  // reset the game
  clearInterval(enemyInterval);

  // reset enemy position
  enemyPosition.value = 0;

  // get random arrow from arrows array
  currentArrow.value = arrows[Math.floor(Math.random() * arrows.length)];

  enemyInterval = setInterval(() => {
    //if the enemy position is less than 50, put an arrow character on it
    if (enemyPosition.value < 50) {
      enemy.value.innerHTML = currentArrow.value.code;
    } else {
      enemy.value.innerHTML = "";
    }
    enemy.value.innerHTML += enemyPosition.value;

    enemyPosition.value += 5;

    // if enemy reaches left side of the screen
    if (enemyPosition.value >= 100) {
      enemyPosition.value = 0;
      console.log("game over");
    }
    // enemys position %
    enemy.value.style.right = `${enemyPosition.value}%`;

    // if enemy position is 70-80, make it white
    if (enemyPosition.value >= 80) {
      enemy.value.style.background = "white";
    } else {
      enemy.value.style.backgroundColor = currentArrow.value.color;
    }
  }, gameSpeed.value);
};

window.addEventListener("keydown", (e) => {
  // ...
  if (e.code === currentArrow.value.keyCode && enemyPosition.value > 80) {
    // gameSpeed.value = gameSpeed.value * 0.9;
    hit.value = true;
  } else {
    // gameSpeed.value = gameSpeed.value * 1.1;
  }
});

const hit = ref(false);
watch(hit, (value) => {
  if (value) {
    // if hit is true, reset enemy position. (the )
    enemyPosition.value = -15;
    console.log("hit");
    hit.value = false;
    arrows.forEach((arrow) => {
      if (arrow.keyCode === currentArrow.value.keyCode) {
        arrow.defeated++;
        console.log("arrow.defeated: ", arrow.defeated);
      }
    });
    // set random arrow on enemy
    currentArrow.value = arrows[Math.floor(Math.random() * arrows.length)];

    newGame();
  }
});

watch(gameSpeed, (value) => {
  // console.log(gameSpeed.value);
  // newGame();
  // keep the game speed between 20 and 100
  // if (gameSpeed.value < 20) {
  //   gameSpeed.value = 20;
  // } else if (gameSpeed.value > 100) {
  //   gameSpeed.value = 100;
  // }
});
const player = ref(null);

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
