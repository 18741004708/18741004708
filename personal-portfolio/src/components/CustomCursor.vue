<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const cursorX = ref(0)
const cursorY = ref(0)
const isHovering = ref(false)
const isClicking = ref(false)

const handleMouseMove = (e) => {
  cursorX.value = e.clientX
  cursorY.value = e.clientY
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

onMounted(() => {
  document.addEventListener('mousemove', handleMouseMove)
  document.addEventListener('mousedown', handleMouseDown)
  document.addEventListener('mouseup', handleMouseUp)
  document.addEventListener('mouseover', handleMouseOver)
})

onUnmounted(() => {
  document.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mousedown', handleMouseDown)
  document.removeEventListener('mouseup', handleMouseUp)
  document.removeEventListener('mouseover', handleMouseOver)
})
</script>

<template>
  <div class="custom-cursor">
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
    <div 
      class="cursor-trail"
      v-for="i in 5"
      :key="i"
      :style="{ 
        left: `${cursorX}px`, 
        top: `${cursorY}px`,
        animationDelay: `${i * 0.05}s`
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
}

.cursor-ring {
  position: absolute;
  width: 40px;
  height: 40px;
  border: 2px solid var(--accent-cyan);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: all 0.15s ease;
  opacity: 0.5;
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

.cursor-trail {
  position: absolute;
  width: 4px;
  height: 4px;
  background: var(--accent-green);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  opacity: 0;
  animation: trail 0.5s ease forwards;
}

@keyframes trail {
  0% {
    opacity: 0.8;
    transform: translate(-50%, -50%) scale(1);
  }
  100% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0);
  }
}

@media (max-width: 768px) {
  .custom-cursor {
    display: none;
  }
}
</style>
