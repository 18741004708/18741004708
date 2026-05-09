<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const canvasRef = ref(null)
let animationId = null
let particles = []
let mouseX = 0
let mouseY = 0
let isMouseInCanvas = false

class Particle {
  constructor(canvas) {
    this.canvas = canvas
    this.reset()
  }
  
  reset() {
    this.x = Math.random() * this.canvas.width
    this.y = Math.random() * this.canvas.height
    this.size = Math.random() * 1.5 + 0.5
    this.speedX = (Math.random() - 0.5) * 0.3
    this.speedY = (Math.random() - 0.5) * 0.3
    this.opacity = Math.random() * 0.3 + 0.1
  }
  
  update() {
    this.x += this.speedX
    this.y += this.speedY
    
    if (isMouseInCanvas) {
      const dx = mouseX - this.x
      const dy = mouseY - this.y
      const distance = Math.sqrt(dx * dx + dy * dy)
      
      if (distance < 80) {
        const force = (80 - distance) / 80
        this.x -= dx * force * 0.01
        this.y -= dy * force * 0.01
      }
    }
    
    if (this.x < 0 || this.x > this.canvas.width) this.speedX *= -1
    if (this.y < 0 || this.y > this.canvas.height) this.speedY *= -1
  }
  
  draw(ctx) {
    ctx.beginPath()
    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(0, 212, 255, ${this.opacity})`
    ctx.fill()
  }
}

const initParticles = (canvas) => {
  particles = []
  const particleCount = Math.min(60, Math.floor((canvas.width * canvas.height) / 25000))
  
  for (let i = 0; i < particleCount; i++) {
    particles.push(new Particle(canvas))
  }
}

const drawConnections = (ctx) => {
  const maxDistance = 120
  for (let i = 0; i < particles.length; i++) {
    for (let j = i + 1; j < particles.length; j++) {
      const dx = particles[i].x - particles[j].x
      const dy = particles[i].y - particles[j].y
      const distance = Math.sqrt(dx * dx + dy * dy)
      
      if (distance < maxDistance) {
        ctx.beginPath()
        ctx.moveTo(particles[i].x, particles[i].y)
        ctx.lineTo(particles[j].x, particles[j].y)
        ctx.strokeStyle = `rgba(0, 212, 255, ${0.08 * (1 - distance / maxDistance)})`
        ctx.lineWidth = 0.5
        ctx.stroke()
      }
    }
  }
}

const animate = (ctx, canvas) => {
  ctx.clearRect(0, 0, canvas.width, canvas.height)
  
  particles.forEach(particle => {
    particle.update()
    particle.draw(ctx)
  })
  
  drawConnections(ctx)
  
  animationId = requestAnimationFrame(() => animate(ctx, canvas))
}

let resizeTimeout
const handleResize = () => {
  clearTimeout(resizeTimeout)
  resizeTimeout = setTimeout(() => {
    const canvas = canvasRef.value
    if (!canvas) return
    
    canvas.width = window.innerWidth
    canvas.height = window.innerHeight
    initParticles(canvas)
  }, 100)
}

const handleMouseMove = (e) => {
  mouseX = e.clientX
  mouseY = e.clientY
}

const handleMouseEnter = () => {
  isMouseInCanvas = true
}

const handleMouseLeave = () => {
  isMouseInCanvas = false
}

onMounted(() => {
  const canvas = canvasRef.value
  if (!canvas) return
  
  const ctx = canvas.getContext('2d')
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  
  initParticles(canvas)
  animate(ctx, canvas)
  
  window.addEventListener('resize', handleResize, { passive: true })
  window.addEventListener('mousemove', handleMouseMove, { passive: true })
  document.addEventListener('mouseenter', handleMouseEnter)
  document.addEventListener('mouseleave', handleMouseLeave)
})

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId)
  }
  window.removeEventListener('resize', handleResize)
  window.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mouseenter', handleMouseEnter)
  document.removeEventListener('mouseleave', handleMouseLeave)
})
</script>

<template>
  <canvas ref="canvasRef" class="particle-canvas"></canvas>
</template>

<style scoped>
.particle-canvas {
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
}
</style>
