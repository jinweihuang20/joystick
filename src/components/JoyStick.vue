<template>
  <div class="joystick">
    <div
      class="stick"
      :style="{transform: 'translate(' + xPos + 'px, ' + yPos + 'px)'}"
      @mousedown="startDragging"
      @touchstart.prevent="startDragging"
    ></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dragging: false,
      xPos: 0,
      yPos: 0,
      centerX: 0,
      centerY: 0,
      maxDistance: 50,
    };
  },
  methods: {
    startDragging(e) {
      e.preventDefault();
      this.dragging = true;
      if (e.type === 'mousedown') {
        document.addEventListener('mousemove', this.handleMouseMove);
        document.addEventListener('mouseup', this.handleMouseUp);
      } else if (e.type === 'touchstart') {
        document.addEventListener('touchmove', this.handleMouseMove);
        document.addEventListener('touchend', this.handleMouseUp);
      }
    },
    handleMouseMove(e) {
      if (this.dragging) {
        const rect = this.$el.getBoundingClientRect();
        const x = e.clientX || e.touches[0].clientX;
        const y = e.clientY || e.touches[0].clientY;
        this.xPos = x - rect.left - this.centerX;
        this.yPos = y - rect.top - this.centerY;
        const distance = Math.sqrt(Math.pow(this.xPos, 2) + Math.pow(this.yPos, 2));
        if (distance > this.maxDistance) {
          this.xPos = (this.xPos * this.maxDistance) / distance;
          this.yPos = (this.yPos * this.maxDistance) / distance;
        }
      }
    },
    handleMouseUp(e) {
      this.dragging = false;
      this.xPos = 0;
      this.yPos = 0;
      if (e.type === 'mouseup') {
        document.removeEventListener('mousemove', this.handleMouseMove);
        document.removeEventListener('mouseup', this.handleMouseUp);
      } else if (e.type === 'touchend') {
        document.removeEventListener('touchmove', this.handleMouseMove);
        document.removeEventListener('touchend', this.handleMouseUp);
      }
    },
  },
  mounted() {
    const rect = this.$el.getBoundingClientRect();
    this.centerX = rect.width / 2;
    this.centerY = rect.height / 2;
  },
};
</script>

<style scoped>
.joystick {
  width: 130px;
  height: 130px;
  border-radius: 50%;
  background-color: lightgray;
  display: flex;
  justify-content: center;
  align-items: center;
}

.stick {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  background-color: rgb(42, 61, 170);
  cursor: pointer;
  user-select: none;
}
</style>
