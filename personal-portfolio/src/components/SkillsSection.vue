<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { useI18n } from 'vue-i18n'

gsap.registerPlugin(ScrollTrigger)
const { t } = useI18n()

const sectionRef = ref(null)

const skillCategories = [
  {
    titleKey: 'skills.frontend',
    icon: 'M4 6h16M4 12h16M4 18h16',
    skills: [
      { name: 'Vue.js / React', level: 95 },
      { name: 'TypeScript', level: 90 },
      { name: 'HTML5 / CSS3', level: 95 },
      { name: 'Tailwind CSS', level: 90 }
    ]
  },
  {
    titleKey: 'skills.backend',
    icon: 'M5 12h14M12 5l7 7-7 7',
    skills: [
      { name: 'Node.js', level: 90 },
      { name: 'Python', level: 85 },
      { name: 'PostgreSQL / MongoDB', level: 85 },
      { name: 'REST API / GraphQL', level: 90 }
    ]
  },
  {
    titleKey: 'skills.devops',
    icon: 'M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5',
    skills: [
      { name: 'Git / GitHub', level: 95 },
      { name: 'Docker', level: 80 },
      { name: 'CI/CD', level: 85 },
      { name: 'AWS / Cloud', level: 80 }
    ]
  }
]

onMounted(() => {
  gsap.fromTo('.skill-category',
    { opacity: 0, y: 50 },
    { 
      opacity: 1, 
      y: 0, 
      duration: 0.8,
      stagger: 0.2,
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 70%',
        toggleActions: 'play none none reverse'
      }
    }
  )
})
</script>

<template>
  <section id="skills" class="skills-section section" ref="sectionRef">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">{{ t('skills.title') }}</h2>
        <p class="section-subtitle">{{ t('skills.subtitle') }}</p>
      </div>
      
      <div class="skills-grid">
        <div 
          v-for="(category, index) in skillCategories" 
          :key="category.titleKey"
          class="skill-category"
          :style="{ animationDelay: `${index * 0.1}s` }"
        >
          <div class="category-header">
            <div class="category-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path :d="category.icon"/>
              </svg>
            </div>
            <h3 class="category-title">{{ t(category.titleKey) }}</h3>
          </div>
          
          <div class="skills-list">
            <div 
              v-for="skill in category.skills" 
              :key="skill.name"
              class="skill-item"
            >
              <div class="skill-info">
                <span class="skill-name">{{ skill.name }}</span>
                <span class="skill-level">{{ skill.level }}%</span>
              </div>
              <div class="skill-bar">
                <div 
                  class="skill-progress"
                  :style="{ width: `${skill.level}%` }"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.skills-section {
  position: relative;
}

.section-header {
  text-align: center;
  margin-bottom: 60px;
}

.skills-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
}

.skill-category {
  background: var(--bg-secondary);
  border-radius: var(--border-radius);
  padding: 30px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.3s ease;
}

.skill-category:hover {
  border-color: var(--accent-cyan);
  transform: translateY(-5px);
  box-shadow: var(--shadow-glow);
}

.category-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 24px;
}

.category-icon {
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--gradient-primary);
  border-radius: 12px;
}

.category-icon svg {
  width: 24px;
  height: 24px;
  color: var(--bg-primary);
}

.category-title {
  font-size: 1.25rem;
  font-weight: 600;
  color: var(--text-primary);
}

.skills-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.skill-item {
  position: relative;
}

.skill-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
}

.skill-name {
  font-size: 0.875rem;
  color: var(--text-secondary);
}

.skill-level {
  font-size: 0.875rem;
  font-weight: 600;
  color: var(--accent-cyan);
  font-family: 'JetBrains Mono', monospace;
}

.skill-bar {
  height: 6px;
  background: var(--bg-tertiary);
  border-radius: 3px;
  overflow: hidden;
}

.skill-progress {
  height: 100%;
  background: var(--gradient-primary);
  border-radius: 3px;
  transition: width 1s ease;
  position: relative;
}

@media (max-width: 968px) {
  .skills-grid {
    grid-template-columns: 1fr;
  }
}
</style>
