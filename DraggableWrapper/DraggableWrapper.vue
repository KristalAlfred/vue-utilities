<template>
    <div ref="draggable" class="draggable" @mousedown="onMouseDown">
        <slot></slot>
    </div>
</template>

<script>
export default {
    data() {
        return {
            positions: {
                currentX: undefined,
                currentY: undefined,
                deltaX: 0,
                deltaY: 0
            }
        }
    },
    methods: {
        onMouseDown(event) {
            event.preventDefault()
            // Get mouse drag starting position
            this.positions.currentX = event.clientX;
            this.positions.currentY = event.clientY;

            // Supply our drag handler until mouse is released
            document.onmouseup = this.onMouseUp
            document.onmousemove = this.onMouseMove
        },
        onMouseMove(event) {
            event.preventDefault()
            // See how far we have moved since the last call...
            this.positions.deltaX = this.positions.currentX - event.clientX
            this.positions.deltaY = this.positions.currentY - event.clientY
            this.positions.currentX = event.clientX
            this.positions.currentY = event.clientY

            // ...and update the position of our component!
            this.$refs.draggable.style.top = (this.$refs.draggable.offsetTop - this.positions.deltaY) + 'px'
            this.$refs.draggable.style.left = (this.$refs.draggable.offsetLeft - this.positions.deltaX) + 'px'
        },
        onMouseUp() {
            document.onmouseup = null
            document.onmousemove = null
        }
    }
}
</script>

<style scoped>

.draggable {
    position: absolute;
}

</style>