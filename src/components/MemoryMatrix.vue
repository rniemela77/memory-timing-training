<template>
  <div class="screen">
    <div class="horizon" ref="horizon"></div>
    <div class="road" ref="road"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed, watchEffect } from "vue";

const horizon = ref(null);
const road = ref(null);

onMounted(() => {
  // start horizon tilt
  //   tiltHorizon();
});

/* every 1 second, tilt the horizon by 1 degree in a random
   direction, increasing the rate as it becomes more tilted*/
const tiltHorizon = () => {
  let tilt = 0;
  let tiltDirection = 1;
  setInterval(() => {
    tilt += tiltDirection;
    if (tilt > 10 || tilt < -10) {
      tiltDirection *= -0.8;
    }
    horizon.value.style.transform = `rotate(${tilt}deg)`;
  }, 100);
};

// start road animation
</script>

<style scoped>
.screen {
  position: fixed;
  width: 100%;
  height: 100vh;
  top: 0;
  left: 0;
  background-color: #000;
  overflow: hidden;
}

.horizon {
  position: fixed;
  bottom: 0;
  top: 50%;
  height: 1px;
  width: 100%;
  background-color: #fff;
  transform-origin: center;
}

.road {
  position: fixed;
  background-color: rgb(0, 63, 100);
  width: 20%;
  height: 50%;
  bottom: 0;
  left: 40%;
  /* triangle */

  animation: road 4s cubic-bezier(1, 0, 1, 0) infinite;
  transition: all 500ms cubic-bezier(1, 0, 1, 0); /* custom */
  transition-timing-function: cubic-bezier(1, 0, 1, 0); /* custom */
}

@keyframes road {
  0% {
    clip-path: polygon(50% 0, 50% 0, 50% 0, 50% 0);
    background-color: red;
  }
  25% {
    clip-path: polygon(50% 0, 50% 0, 100% 100%, 0% 100%);
    background-color: white;
  }
  500% {
    clip-path: polygon(0 100%, 100% 100%, 100% 100%, 0% 100%);
    background-color: blue;
  }
}
</style>
