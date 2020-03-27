<template>
  <div class="about">
    <h1>This is an about page</h1>
    <div class="canvas" ref="canvas" @mousemove="moveMouse" @mouseup="endDrag">
      <Box v-for="(box, i) in boxes" :key="i"
        :box="box" @startDrag="startDrag" />
    </div>
  </div>
</template>

<script>
import Box from '@/components/Box'
export default {
  components: {
    Box,
  },
  data() {
    return {
      boxes: [
        {
          top: 0,
          left: 0,
          width: 100,
          height: 100
        },
        {
          top: 250,
          left: 0,
          width: 100,
          height: 100
        },
        {
          top: 400,
          left: 0,
          width: 100,
          height: 100
        },
      ],
      dragging: {
        box: null,
        mode: null,
        originX: null,
        originY: null
      },
      preX: null,
      preY: null
    }
  },
  mounted () {
    this.$refs.canvas.onmousemove = this.moveMouse;
  },
  methods: {
    startDrag(startX, startY, mode, box) {
      this.dragging.box = box
      this.dragging.mode = mode
      this.dragging.originX = startX
      this.dragging.originY = startY
    },
    endDrag() {
      this.dragging.box = null
      this.dragging.mode = null
      this.dragging.originX = null
      this.dragging.originY = null
    },
    moveMouse(event) {
      if (this.dragging.box) {
        const moveX = event.clientX - this.dragging.originX
        const moveY = event.clientY - this.dragging.originY

        if (this.dragging.mode === 'move'){
          this.dragging.box.top += moveY
          this.dragging.box.left += moveX
        } else if (this.dragging.mode === 'top-left'){
          this.dragging.box.top += moveY
          this.dragging.box.left += moveX
          this.dragging.box.height -= moveY
          this.dragging.box.width -= moveX
        } else if (this.dragging.mode === 'top-right'){
          this.dragging.box.top += moveY
          this.dragging.box.height -= moveY
          this.dragging.box.width += moveX
        } else if (this.dragging.mode === 'bottom-left'){
          this.dragging.box.left += moveX
          this.dragging.box.height += moveY
          this.dragging.box.width -= moveX
        } else if (this.dragging.mode === 'bottom-right'){
          this.dragging.box.height += moveY
          this.dragging.box.width += moveX
        } else if (this.dragging.mode === 'top'){
          this.dragging.box.top += moveY
          this.dragging.box.height -= moveY
        } else if (this.dragging.mode === 'left'){
          this.dragging.box.left += moveX
          this.dragging.box.width -= moveX
        } else if (this.dragging.mode === 'right'){
          this.dragging.box.width += moveX
        } else if (this.dragging.mode === 'bottom'){
          this.dragging.box.height += moveY
        }

        this.dragging.originY = event.clientY
        this.dragging.originX = event.clientX
      }
    }
  }
}
</script>

<style>
.canvas {
  position: relative;
  width: 600px;
  height: 600px;
  background-color: gray;
}
</style>