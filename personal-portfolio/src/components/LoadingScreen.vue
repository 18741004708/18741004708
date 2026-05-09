<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'

const emit = defineEmits(['complete'])
const progress = ref(0)
const loadingText = ref('Initializing...')

const loadingSteps = [
  { progress: 20, text: 'Loading assets...' },
  { progress: 40, text: 'Preparing animations...' },
  { progress: 60, text: 'Setting up particles...' },
  { progress: 80, text: 'Almost ready...' },
  { progress: 100, text: 'Welcome!' }
]

onMounted(() => {
  const tl = gsap.timeline()
  
  loadingSteps.forEach((step, index) => {
    tl.to(progress, {
      value: step.progress,
      duration: 0.4,
      delay: index === 0 ? 0.2 : 0.1,
      onUpdate: () => {
        loadingText.value = step.text
      }
    })
  })
  
  tl.to('.loading-screen', {
    opacity: 0,
    duration: 0.5,
    delay: 0.3,
    onComplete: () => {
      emit('complete')
    }
  })
})
</script>

<template>
  <div class="loading-screen">
    <div class="loading-content">
      <div class="loading-logo">
        <div class="logo-circle"></div>
        <div class="logo-circle"></div>
        <div class="logo-circle"></div>
      </div>
      
      <div class="loading-bar-container">
        <div class="loading-bar" :style="{ width: `${progress}%` }"></div>
      </div>
      
      <p class="loading-text">{{ loadingText }}</p>
      <p class="loading-percentage">{{ Math.round(progress) }}%</p>
    </div>
    
    <div class="loading-grid">
      <div v-for="i in 50" :key="i" class="grid-cell" :style="{ animationDelay: `${i * 0.02}s` }"></div>
    </div>
  </div>
</template>

<style scoped>
.loading-screen {
  position: fixed;
  inset: 0;
  background: var(--bg-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  overflow: hidden;
}

.loading-content {
  text-align: center;
  z-index: 1;
}

.loading-logo {
  display: flex;
  gap: 12px;
  justify-content: center;
  margin-bottom: 40px;
}

.logo-circle {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: var(--accent-cyan);
  animation: bounce 0.6s ease-in-out infinite;
}

.logo-circle:nth-child(2) {
  animation-delay: 0.1s;
  background: var(--accent-green);
}

.logo-circle:nth-child(3) {
  animation-delay: 0.2s;
  background: var(--accent-orange);
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-20px); }
}

.loading-bar-container {
  width: 200px;
  height: 4px;
  background: var(--bg-tertiary);
  border-radius: 2px;
  overflow: hidden;
  margin: 0 auto 20px;
}

.loading-bar {
  height: 100%;
  background: var(--gradient-primary);
  border-radius: 2px;
  transition: width 0.3s ease;
}

.loading-text {
  font-size: 0.875rem;
  color: var(--text-secondary);
  margin-bottom: 8px;
}

.loading-percentage {
  font-family: 'JetBrains Mono', monospace;
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--text-primary);
}

.loading-grid {
  position: absolute;
  inset: 0;
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  gap: 2px;
  padding: 2px;
  opacity: 0.1;
}

.grid-cell {
  background: var(--accent-cyan);
  animation: fadeIn 0.5s ease forwards;
  opacity: 0;
}

@keyframes fadeIn {
  to { opacity: 1; }
}
</style>
