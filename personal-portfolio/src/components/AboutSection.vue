<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { useI18n } from 'vue-i18n'

gsap.registerPlugin(ScrollTrigger)
const { t } = useI18n()
const sectionRef = ref(null)

onMounted(() => {
  gsap.fromTo('.about-content', 
    { opacity: 0, x: -50 },
    { 
      opacity: 1, 
      x: 0, 
      duration: 1,
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 70%',
        end: 'bottom 30%',
        toggleActions: 'play none none reverse'
      }
    }
  )
  
  gsap.fromTo('.about-image',
    { opacity: 0, x: 50 },
    { 
      opacity: 1, 
      x: 0, 
      duration: 1,
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 70%',
        end: 'bottom 30%',
        toggleActions: 'play none none reverse'
      }
    }
  )
})
</script>

<template>
  <section id="about" class="about-section section" ref="sectionRef">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">{{ t('about.title') }}</h2>
        <p class="section-subtitle">{{ t('about.subtitle') }}</p>
      </div>
      
      <div class="about-grid">
        <div class="about-content">
          <div class="about-text">
            <p class="about-intro">{{ t('about.intro') }}</p>
            <p>{{ t('about.paragraph1') }}</p>
            <p>{{ t('about.paragraph2') }}</p>
          </div>
          
          <div class="about-highlights">
            <div class="highlight-item">
              <div class="highlight-icon">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
                </svg>
              </div>
              <div class="highlight-text">
                <h4>{{ t('about.fullStack') }}</h4>
                <p>{{ t('about.fullStackDesc') }}</p>
              </div>
            </div>
            
            <div class="highlight-item">
              <div class="highlight-icon">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <circle cx="12" cy="12" r="10"/>
                  <path d="M12 6v6l4 2"/>
                </svg>
              </div>
              <div class="highlight-text">
                <h4>{{ t('about.fastDelivery') }}</h4>
                <p>{{ t('about.fastDeliveryDesc') }}</p>
              </div>
            </div>
            
            <div class="highlight-item">
              <div class="highlight-icon">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/>
                  <circle cx="9" cy="7" r="4"/>
                  <path d="M23 21v-2a4 4 0 0 0-3-3.87M16 3.13a4 4 0 0 1 0 7.75"/>
                </svg>
              </div>
              <div class="highlight-text">
                <h4>{{ t('about.collaborative') }}</h4>
                <p>{{ t('about.collaborativeDesc') }}</p>
              </div>
            </div>
          </div>
        </div>
        
        <div class="about-image">
          <div class="image-container">
            <div class="image-frame">
              <div class="code-window">
                <div class="window-header">
                  <span class="dot red"></span>
                  <span class="dot yellow"></span>
                  <span class="dot green"></span>
                </div>
                <div class="window-content">
                  <pre><code><span class="keyword">const</span> <span class="variable">developer</span> = {
  <span class="property">name</span>: <span class="string">"Your Name"</span>,
  <span class="property">role</span>: <span class="string">"Full Stack Dev"</span>,
  <span class="property">skills</span>: [
    <span class="string">"JavaScript"</span>,
    <span class="string">"Vue.js"</span>,
    <span class="string">"Node.js"</span>,
    <span class="string">"Python"</span>
  ],
  <span class="property">passion</span>: <span class="string">"Building things"</span>,
  <span class="method">createAwesome</span>: () => <span class="keyword">true</span>
};</code></pre>
                </div>
              </div>
            </div>
            <div class="image-decoration"></div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.about-section {
  background: var(--bg-secondary);
  position: relative;
  overflow: hidden;
}

.about-section::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--accent-cyan), transparent);
}

.section-header {
  text-align: center;
  margin-bottom: 60px;
}

.about-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 80px;
  align-items: center;
}

.about-text {
  margin-bottom: 40px;
}

.about-intro {
  font-size: 1.25rem;
  color: var(--text-primary);
  margin-bottom: 20px;
  line-height: 1.8;
}

.about-text p {
  color: var(--text-secondary);
  margin-bottom: 16px;
  line-height: 1.8;
}

.about-highlights {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.highlight-item {
  display: flex;
  gap: 16px;
  padding: 20px;
  background: var(--bg-tertiary);
  border-radius: var(--border-radius);
  transition: all 0.3s ease;
  border: 1px solid transparent;
}

.highlight-item:hover {
  border-color: var(--accent-cyan);
  transform: translateX(10px);
}

.highlight-icon {
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--gradient-primary);
  border-radius: 12px;
  flex-shrink: 0;
}

.highlight-icon svg {
  width: 24px;
  height: 24px;
  color: var(--bg-primary);
}

.highlight-text h4 {
  font-size: 1rem;
  font-weight: 600;
  margin-bottom: 4px;
  color: var(--text-primary);
}

.highlight-text p {
  font-size: 0.875rem;
  color: var(--text-muted);
  margin: 0;
}

.image-container {
  position: relative;
}

.image-frame {
  position: relative;
  z-index: 1;
}

.code-window {
  background: var(--bg-primary);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: var(--shadow-card);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.window-header {
  display: flex;
  gap: 8px;
  padding: 12px 16px;
  background: var(--bg-tertiary);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}

.dot.red { background: #ff5f56; }
.dot.yellow { background: #ffbd2e; }
.dot.green { background: #27ca40; }

.window-content {
  padding: 20px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.875rem;
  line-height: 1.8;
  overflow-x: auto;
}

.window-content code {
  color: var(--text-secondary);
}

.keyword { color: var(--accent-orange); }
.variable { color: var(--accent-cyan); }
.property { color: var(--accent-green); }
.string { color: #ffd93d; }
.method { color: var(--accent-cyan); }

.image-decoration {
  position: absolute;
  top: -20px;
  right: -20px;
  width: 100%;
  height: 100%;
  border: 2px solid var(--accent-cyan);
  border-radius: 12px;
  opacity: 0.3;
  z-index: 0;
}

@media (max-width: 968px) {
  .about-grid {
    grid-template-columns: 1fr;
    gap: 40px;
  }
  
  .about-image {
    order: -1;
  }
}
</style>
