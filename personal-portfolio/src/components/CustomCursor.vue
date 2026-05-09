<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const cursorX = ref(0)
const cursorY = ref(0)
const isHovering = ref(false)
const isClicking = ref(false)
const isVisible = ref(false)

let rafId = null
let targetX = 0
let targetY = 0
let currentX = 0
let currentY = 0

const lerp = (start, end, factor) => start + (end - start) * factor

const updateCursor = () => {
  currentX = lerp(currentX, targetX, 0.15)
  currentY = lerp(currentY, targetY, 0.15)
  
  cursorX.value = currentX
  cursorY.value = currentY
  
  rafId = requestAnimationFrame(updateCursor)
}

const handleMouseMove = (e) => {
  targetX = e.clientX
  targetY = e.clientY
  if (!isVisible.value) {
    isVisible.value = true
    currentX = targetX
    currentY = targetY
  }
}

const handleMouseDown = () => {
  isClicking.value = true
}

const handleMouseUp = () => {
  isClicking.value = false
}

const handleMouseOver = (e) => {
  if (e.target.closest('a, button, .clickable')) {
    isHovering.value = true
  } else {
    isHovering.value = false
  }
}

const handleMouseLeave = () => {
  isVisible.value = false
}

onMounted(() => {
  const isTouchDevice = 'ontouchstart' in window || navigator.maxTouchPoints > 0
  if (isTouchDevice) return
  
  document.addEventListener('mousemove', handleMouseMove, { passive: true })
  document.addEventListener('mousedown', handleMouseDown)
  document.addEventListener('mouseup', handleMouseUp)
  document.addEventListener('mouseover', handleMouseOver)
  document.addEventListener('mouseleave', handleMouseLeave)
  
  rafId = requestAnimationFrame(updateCursor)
})

onUnmounted(() => {
  if (rafId) {
    cancelAnimationFrame(rafId)
  }
  document.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mousedown', handleMouseDown)
  document.removeEventListener('mouseup', handleMouseUp)
  document.removeEventListener('mouseover', handleMouseOver)
  document.removeEventListener('mouseleave', handleMouseLeave)
})
</script>

<template>
  <div class="custom-cursor" v-show="isVisible">
    <div 
      class="cursor-dot"
      :style="{ 
        left: `${cursorX}px`, 
        top: `${cursorY}px`,
        transform: `translate(-50%, -50%) scale(${isClicking ? 0.5 : 1})`
      }"
    ></div>
    <div 
      class="cursor-ring"
      :class="{ hovering: isHovering, clicking: isClicking }"
      :style="{ 
        left: `${cursorX}px`, 
        top: `${cursorY}px`
      }"
    ></div>
  </div>
</template>

<style scoped>
.custom-cursor {
  pointer-events: none;
  position: fixed;
  inset: 0;
  z-index: 10000;
}

.cursor-dot {
  position: absolute;
  width: 8px;
  height: 8px;
  background: var(--accent-cyan);
  border-radius: 50%;
  transition: transform 0.1s ease;
  box-shadow: 0 0 10px var(--accent-cyan);
  will-change: transform, left, top;
}

.cursor-ring {
  position: absolute;
  width: 40px;
  height: 40px;
  border: 2px solid var(--accent-cyan);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: width 0.2s ease, height 0.2s ease, border-color 0.2s ease, opacity 0.2s ease;
  opacity: 0.5;
  will-change: transform, left, top;
}

.cursor-ring.hovering {
  width: 60px;
  height: 60px;
  border-color: var(--accent-green);
  opacity: 0.8;
}

.cursor-ring.clicking {
  width: 30px;
  height: 30px;
  opacity: 1;
}

@media (max-width: 768px), (hover: none) {
  .custom-cursor {
    display: none;
  }
}
</style>
