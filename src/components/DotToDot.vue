<template>
  <div
    id="canvas"
    @mousedown="startLine"
    @mousemove="drawLine"
    @mouseup="endLine"
    style="
      position: relative;
      height: 500px;
      width: 500px;
      border: 1px solid black;
    "
  >
    <div
      v-for="(line, index) in lines"
      :key="index"
      :style="{
        position: 'absolute',
        top: line.start.y + 'px',
        left: line.start.x + 'px',
        width: line.length + 'px',
        height: '1px',
        backgroundColor: 'black',
        transform: 'rotate(' + line.angle + 'rad)',
        transformOrigin: '0 0',
      }"
    ></div>
  </div>
</template>
<script src="https://cdnjs.cloudflare.com/ajax/libs/phaser/3.55.2/phaser.min.js"></script>

<script>
export default {
  data() {
    return {
      lines: [],
      currentLine: null,
    };
  },
  methods: {
    startLine(event) {
      this.currentLine = {
        start: { x: event.clientX, y: event.clientY },
        end: { x: event.clientX, y: event.clientY },
      };
    },
    drawLine(event) {
      if (this.currentLine) {
        this.currentLine.end = { x: event.clientX, y: event.clientY };

        // Calculate length and angle for CSS
        let dx = this.currentLine.end.x - this.currentLine.start.x;
        let dy = this.currentLine.end.y - this.currentLine.start.y;
        this.currentLine.length = Math.sqrt(dx * dx + dy * dy);
        this.currentLine.angle = Math.atan2(dy, dx);
      }
    },
    endLine(event) {
      if (this.currentLine) {
        this.lines.push({ ...this.currentLine });
        this.currentLine = null;
      }
    },
  },
};
</script>

<style scoped>
#canvas {
  position: fixed;
  height: 100%;
  width: 100%;
  border: 1px solid black;
}
</style>
