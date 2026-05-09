<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { gsap } from 'gsap'
import { useI18n } from 'vue-i18n'

const { t } = useI18n()
const heroRef = ref(null)
const leftEyeRef = ref(null)
const rightEyeRef = ref(null)
const characterRef = ref(null)

let mouseX = 0
let mouseY = 0
let targetEyeX = 0
let targetEyeY = 0
let currentEyeX = 0
let currentEyeY = 0
let rafId = null

const lerp = (start, end, factor) => start + (end - start) * factor

const updateEyes = () => {
  currentEyeX = lerp(currentEyeX, targetEyeX, 0.1)
  currentEyeY = lerp(currentEyeY, targetEyeY, 0.1)
  
  if (leftEyeRef.value) {
    leftEyeRef.value.style.transform = `translate(${currentEyeX}px, ${currentEyeY}px)`
  }
  if (rightEyeRef.value) {
    rightEyeRef.value.style.transform = `translate(${currentEyeX}px, ${currentEyeY}px)`
  }
  
  rafId = requestAnimationFrame(updateEyes)
}

const handleMouseMove = (e) => {
  if (!heroRef.value) return
  
  const rect = heroRef.value.getBoundingClientRect()
  const centerX = rect.left + rect.width / 2
  const centerY = rect.top + rect.height / 2
  
  mouseX = e.clientX - centerX
  mouseY = e.clientY - centerY
  
  const maxMove = 6
  const angle = Math.atan2(mouseY, mouseX)
  const distance = Math.min(Math.sqrt(mouseX ** 2 + mouseY ** 2) / 30, maxMove)
  
  targetEyeX = Math.cos(angle) * distance
  targetEyeY = Math.sin(angle) * distance
  
  if (characterRef.value) {
    const maxTilt = 3
    const tiltX = (mouseY / window.innerHeight) * maxTilt
    const tiltY = -(mouseX / window.innerWidth) * maxTilt
    characterRef.value.style.transform = `rotateX(${tiltX}deg) rotateY(${tiltY}deg)`
  }
}

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove, { passive: true })
  rafId = requestAnimationFrame(updateEyes)
  
  gsap.fromTo('.hero-content', 
    { opacity: 0, y: 50 },
    { opacity: 1, y: 0, duration: 1, delay: 0.5 }
  )
  
  gsap.fromTo('.hero-character',
    { opacity: 0, scale: 0.8 },
    { opacity: 1, scale: 1, duration: 1, delay: 0.8 }
  )
})

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove)
  if (rafId) cancelAnimationFrame(rafId)
})
</script>

<template>
  <section id="hero" class="hero-section" ref="heroRef">
    <div class="container hero-container">
      <div class="hero-content">
        <div class="hero-badge">
          <span class="badge-dot"></span>
          {{ t('hero.badge') }}
        </div>
        
        <h1 class="hero-title">
          <span class="title-line">{{ t('hero.greeting') }}</span>
          <span class="title-highlight">{{ t('hero.title') }}</span>
        </h1>
        
        <p class="hero-description">
          {{ t('hero.description') }}
        </p>
        
        <div class="hero-actions">
          <button class="btn btn-primary clickable" onclick="document.getElementById('projects').scrollIntoView({behavior: 'smooth'})">
            {{ t('hero.viewWork') }}
            <span class="btn-arrow">-></span>
          </button>
          <button class="btn btn-secondary clickable" onclick="document.getElementById('contact').scrollIntoView({behavior: 'smooth'})">
            {{ t('hero.getInTouch') }}
          </button>
        </div>
        
        <div class="hero-stats">
          <div class="stat">
            <span class="stat-number">5+</span>
            <span class="stat-label">{{ t('hero.yearsExperience') }}</span>
          </div>
          <div class="stat">
            <span class="stat-number">50+</span>
            <span class="stat-label">{{ t('hero.projectsCompleted') }}</span>
          </div>
          <div class="stat">
            <span class="stat-number">30+</span>
            <span class="stat-label">{{ t('hero.happyClients') }}</span>
          </div>
        </div>
      </div>
      
      <div class="hero-character" ref="characterRef">
        <div class="character-container">
          <div class="character-body">
            <div class="character-head">
              <div class="face">
                <div class="eye left-eye">
                  <div class="pupil" ref="leftEyeRef"></div>
                </div>
                <div class="eye right-eye">
                  <div class="pupil" ref="rightEyeRef"></div>
                </div>
                <div class="mouth"></div>
              </div>
            </div>
            <div class="character-torso">
              <div class="laptop">
                <div class="laptop-screen">
                  <div class="code-line"></div>
                  <div class="code-line short"></div>
                  <div class="code-line"></div>
                </div>
              </div>
            </div>
          </div>
          
          <div class="character-glow"></div>
          <div class="floating-elements">
            <div class="float-element el-1">{ }</div>
            <div class="float-element el-2">&lt;/&gt;</div>
            <div class="float-element el-3">;</div>
            <div class="float-element el-4">( )</div>
          </div>
        </div>
      </div>
    </div>
    
    <div class="scroll-indicator">
      <div class="mouse">
        <div class="wheel"></div>
      </div>
      <span>{{ t('hero.scrollExplore') }}</span>
    </div>
  </section>
</template>

<style scoped>
.hero-section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  position: relative;
  padding-top: 80px;
}

.hero-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: center;
}

.hero-content {
  z-index: 1;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: rgba(0, 255, 136, 0.1);
  border: 1px solid rgba(0, 255, 136, 0.3);
  border-radius: 20px;
  font-size: 0.875rem;
  color: var(--accent-green);
  margin-bottom: 24px;
}

.badge-dot {
  width: 8px;
  height: 8px;
  background: var(--accent-green);
  border-radius: 50%;
  animation: pulse 2s ease-in-out infinite;
}

.hero-title {
  margin-bottom: 24px;
}

.title-line {
  display: block;
  font-size: clamp(1.5rem, 3vw, 2rem);
  font-weight: 400;
  color: var(--text-secondary);
  margin-bottom: 8px;
}

.title-highlight {
  display: block;
  font-size: clamp(2.5rem, 6vw, 4.5rem);
  font-weight: 700;
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  line-height: 1.1;
}

.hero-description {
  font-size: 1.125rem;
  color: var(--text-secondary);
  max-width: 500px;
  margin-bottom: 32px;
  line-height: 1.8;
}

.hero-actions {
  display: flex;
  gap: 16px;
  margin-bottom: 48px;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 14px 28px;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.btn-primary {
  background: var(--gradient-primary);
  color: var(--bg-primary);
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 30px rgba(0, 212, 255, 0.3);
}

.btn-arrow {
  transition: transform 0.3s ease;
}

.btn-primary:hover .btn-arrow {
  transform: translateX(4px);
}

.btn-secondary {
  background: transparent;
  color: var(--text-primary);
  border: 2px solid rgba(255, 255, 255, 0.2);
}

.btn-secondary:hover {
  border-color: var(--accent-cyan);
  color: var(--accent-cyan);
}

.hero-stats {
  display: flex;
  gap: 40px;
}

.stat {
  display: flex;
  flex-direction: column;
}

.stat-number {
  font-size: 2rem;
  font-weight: 700;
  color: var(--text-primary);
  font-family: 'JetBrains Mono', monospace;
}

.stat-label {
  font-size: 0.875rem;
  color: var(--text-muted);
}

.hero-character {
  perspective: 1000px;
  transition: transform 0.1s ease-out;
}

.character-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.character-body {
  position: relative;
  z-index: 1;
}

.character-head {
  width: 120px;
  height: 120px;
  background: linear-gradient(135deg, #1a1a25 0%, #12121a 100%);
  border-radius: 50%;
  margin: 0 auto 20px;
  border: 3px solid rgba(0, 212, 255, 0.3);
  box-shadow: 0 0 30px rgba(0, 212, 255, 0.2);
  animation: float 3s ease-in-out infinite;
}

.face {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  padding-top: 10px;
}

.eye {
  width: 24px;
  height: 24px;
  background: white;
  border-radius: 50%;
  position: relative;
  overflow: hidden;
}

.pupil {
  position: absolute;
  width: 12px;
  height: 12px;
  background: var(--bg-primary);
  border-radius: 50%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  will-change: transform;
}

.pupil::after {
  content: '';
  position: absolute;
  width: 4px;
  height: 4px;
  background: var(--accent-cyan);
  border-radius: 50%;
  top: 2px;
  right: 2px;
}

.mouth {
  position: absolute;
  bottom: 25px;
  left: 50%;
  transform: translateX(-50%);
  width: 30px;
  height: 15px;
  border-bottom: 3px solid var(--accent-cyan);
  border-radius: 0 0 50% 50%;
}

.character-torso {
  width: 140px;
  height: 100px;
  background: linear-gradient(135deg, #1a1a25 0%, #12121a 100%);
  border-radius: 20px 20px 40px 40px;
  margin: 0 auto;
  border: 3px solid rgba(0, 255, 136, 0.2);
  display: flex;
  justify-content: center;
  align-items: center;
}

.laptop {
  width: 80px;
  height: 50px;
  background: var(--bg-tertiary);
  border-radius: 4px 4px 0 0;
  border: 2px solid rgba(255, 255, 255, 0.1);
  position: relative;
}

.laptop::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: -10px;
  right: -10px;
  height: 8px;
  background: var(--bg-tertiary);
  border-radius: 0 0 4px 4px;
  border: 2px solid rgba(255, 255, 255, 0.1);
  border-top: none;
}

.laptop-screen {
  padding: 8px;
}

.code-line {
  height: 4px;
  background: var(--accent-cyan);
  border-radius: 2px;
  margin-bottom: 4px;
  opacity: 0.6;
}

.code-line.short {
  width: 60%;
}

.code-line:nth-child(2) {
  background: var(--accent-green);
}

.character-glow {
  position: absolute;
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(0, 212, 255, 0.15) 0%, transparent 70%);
  border-radius: 50%;
  z-index: 0;
  animation: pulse 4s ease-in-out infinite;
}

.floating-elements {
  position: absolute;
  inset: -50px;
  pointer-events: none;
}

.float-element {
  position: absolute;
  font-family: 'JetBrains Mono', monospace;
  font-size: 1.25rem;
  color: var(--accent-cyan);
  opacity: 0.4;
  animation: float 4s ease-in-out infinite;
}

.el-1 { top: 0; left: 0; animation-delay: 0s; }
.el-2 { top: 0; right: 0; animation-delay: 0.5s; color: var(--accent-green); }
.el-3 { bottom: 20px; left: -10px; animation-delay: 1s; color: var(--accent-orange); }
.el-4 { bottom: 0; right: 0; animation-delay: 1.5s; }

.scroll-indicator {
  position: absolute;
  bottom: 40px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  color: var(--text-muted);
  font-size: 0.875rem;
}

.mouse {
  width: 24px;
  height: 40px;
  border: 2px solid var(--text-muted);
  border-radius: 12px;
  position: relative;
}

.wheel {
  width: 4px;
  height: 8px;
  background: var(--accent-cyan);
  border-radius: 2px;
  position: absolute;
  top: 8px;
  left: 50%;
  transform: translateX(-50%);
  animation: scroll 2s ease-in-out infinite;
}

@keyframes scroll {
  0%, 100% { transform: translateX(-50%) translateY(0); opacity: 1; }
  50% { transform: translateX(-50%) translateY(10px); opacity: 0.5; }
}

@media (max-width: 968px) {
  .hero-container {
    grid-template-columns: 1fr;
    text-align: center;
  }
  
  .hero-content {
    order: 2;
  }
  
  .hero-character {
    order: 1;
  }
  
  .hero-description {
    margin: 0 auto 32px;
  }
  
  .hero-actions {
    justify-content: center;
  }
  
  .hero-stats {
    justify-content: center;
  }
  
  .character-head {
    width: 100px;
    height: 100px;
  }
  
  .character-torso {
    width: 120px;
    height: 80px;
  }
}

@media (max-width: 480px) {
  .hero-actions {
    flex-direction: column;
  }
  
  .btn {
    width: 100%;
    justify-content: center;
  }
  
  .hero-stats {
    flex-direction: column;
    gap: 20px;
  }
}
</style>
