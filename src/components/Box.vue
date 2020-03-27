<template>
  <div ref="container" class="box" :style="boxStyle">
      <div class="edge-top-left" @mousedown="startDrag($event, 'top-left')"/>
      <div class="edge-top" @mousedown="startDrag($event, 'top')"/>
      <div class="edge-top-right" @mousedown="startDrag($event, 'top-right')"/>
      <div class="edge-right" @mousedown="startDrag($event, 'right')"/>
      <div class="edge-bottom-right" @mousedown="startDrag($event, 'bottom-right')"/>
      <div class="edge-bottom" @mousedown="startDrag($event, 'bottom')"/>
      <div class="edge-bottom-left" @mousedown="startDrag($event, 'bottom-left')"/>
      <div class="edge-left" @mousedown="startDrag($event, 'left')"/>
      <div class="center" @mousedown="startDrag($event, 'move')"/>
  </div>
</template>

<script>
export default {
    props: {
        box: {
            type: Object,
            required: true
        }
    },
    computed: {
        boxStyle() {
            return [
                {width: this.box.width + 'px'},
                {height: this.box.height + 'px'},
                {top: this.box.top + 'px'},
                {left: this.box.left + 'px'}
            ]
        }
    },
    methods: {
        startDrag(event, mode) {
            this.$emit('startDrag', event.clientX, event.clientY, mode, this.box)
        }
    },
}
</script>

<style lang="scss" scoped>
$handle-width: 12px;

.box, .center {
    position: absolute;
    background-color: yellow;
}
.center {top: $handle-width; left: $handle-width; bottom: $handle-width; right: $handle-width;}
.center:hover {cursor: move;}

.edge-top-left,
.edge-top-right,
.edge-bottom-left,
.edge-bottom-right {
    position: absolute;
    width: $handle-width;
    height: $handle-width;
    background-color: violet;
}

.edge-top-left {top: 0; left: 0;}
.edge-top-right {top: 0; right: 0;}
.edge-bottom-left {bottom: 0; left: 0;}
.edge-bottom-right {bottom: 0; right: 0;}

.edge-top-left:hover,
.edge-bottom-right:hover {cursor: nwse-resize;}
.edge-top-right:hover,
.edge-bottom-left:hover {cursor: nesw-resize;}

.edge-top,
.edge-bottom,
.edge-left,
.edge-right {
    position: absolute;
    background-color: thistle;
}

.edge-top {top: 0; left: $handle-width; right: $handle-width; height: $handle-width;}
.edge-bottom {bottom: 0; left: $handle-width; right: $handle-width; height: $handle-width;}
.edge-left {top: $handle-width; bottom: $handle-width; left: 0; width: $handle-width;}
.edge-right {top: $handle-width; bottom: $handle-width; right: 0; width: $handle-width;}

.edge-top:hover,
.edge-bottom:hover {cursor: ns-resize}
.edge-left:hover,
.edge-right:hover {cursor: ew-resize}

</style>