<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { useI18n } from 'vue-i18n'

gsap.registerPlugin(ScrollTrigger)
const { t } = useI18n()

const sectionRef = ref(null)

const experiences = [
  {
    year: '2023 - Present',
    yearKey: 'experience.present',
    titleKey: 'experience.job1Title',
    companyKey: 'experience.job1Company',
    descKey: 'experience.job1Desc',
    highlights: ['job1Highlight1', 'job1Highlight2', 'job1Highlight3']
  },
  {
    year: '2021 - 2023',
    titleKey: 'experience.job2Title',
    companyKey: 'experience.job2Company',
    descKey: 'experience.job2Desc',
    highlights: ['job2Highlight1', 'job2Highlight2', 'job2Highlight3']
  },
  {
    year: '2019 - 2021',
    titleKey: 'experience.job3Title',
    companyKey: 'experience.job3Company',
    descKey: 'experience.job3Desc',
    highlights: ['job3Highlight1', 'job3Highlight2', 'job3Highlight3']
  },
  {
    year: '2018 - 2019',
    titleKey: 'experience.job4Title',
    companyKey: 'experience.job4Company',
    descKey: 'experience.job4Desc',
    highlights: ['job4Highlight1', 'job4Highlight2', 'job4Highlight3']
  }
]

onMounted(() => {
  gsap.fromTo('.timeline-item',
    { opacity: 0, x: -50 },
    { 
      opacity: 1, 
      x: 0, 
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
  <section id="experience" class="experience-section section" ref="sectionRef">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">{{ t('experience.title') }}</h2>
        <p class="section-subtitle">{{ t('experience.subtitle') }}</p>
      </div>
      
      <div class="timeline">
        <div class="timeline-line"></div>
        
        <div 
          v-for="(exp, index) in experiences"
          :key="index"
          class="timeline-item"
        >
          <div class="timeline-marker">
            <div class="marker-dot"></div>
          </div>
          
          <div class="timeline-content">
            <div class="timeline-year">{{ exp.year }}</div>
            <h3 class="timeline-title">{{ t(exp.titleKey) }}</h3>
            <div class="timeline-company">{{ t(exp.companyKey) }}</div>
            <p class="timeline-description">{{ t(exp.descKey) }}</p>
            
            <ul class="timeline-highlights">
              <li v-for="highlight in exp.highlights" :key="highlight">
                {{ t(`experience.${highlight}`) }}
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.experience-section {
  position: relative;
}

.section-header {
  text-align: center;
  margin-bottom: 60px;
}

.timeline {
  position: relative;
  max-width: 800px;
  margin: 0 auto;
}

.timeline-line {
  position: absolute;
  left: 20px;
  top: 0;
  bottom: 0;
  width: 2px;
  background: linear-gradient(180deg, var(--accent-cyan), var(--accent-green));
}

.timeline-item {
  position: relative;
  padding-left: 60px;
  padding-bottom: 50px;
}

.timeline-item:last-child {
  padding-bottom: 0;
}

.timeline-marker {
  position: absolute;
  left: 12px;
  top: 0;
}

.marker-dot {
  width: 18px;
  height: 18px;
  background: var(--bg-primary);
  border: 3px solid var(--accent-cyan);
  border-radius: 50%;
  transition: all 0.3s ease;
}

.timeline-item:hover .marker-dot {
  background: var(--accent-cyan);
  box-shadow: 0 0 20px var(--accent-cyan);
}

.timeline-content {
  background: var(--bg-secondary);
  border-radius: var(--border-radius);
  padding: 24px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.3s ease;
}

.timeline-item:hover .timeline-content {
  border-color: var(--accent-cyan);
  transform: translateX(10px);
}

.timeline-year {
  display: inline-block;
  padding: 4px 12px;
  background: var(--gradient-primary);
  color: var(--bg-primary);
  font-size: 0.75rem;
  font-weight: 600;
  border-radius: 4px;
  margin-bottom: 12px;
  font-family: 'JetBrains Mono', monospace;
}

.timeline-title {
  font-size: 1.25rem;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 4px;
}

.timeline-company {
  font-size: 0.875rem;
  color: var(--accent-cyan);
  margin-bottom: 12px;
}

.timeline-description {
  font-size: 0.875rem;
  color: var(--text-secondary);
  line-height: 1.6;
  margin-bottom: 16px;
}

.timeline-highlights {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.timeline-highlights li {
  font-size: 0.75rem;
  color: var(--text-muted);
  padding: 4px 10px;
  background: var(--bg-tertiary);
  border-radius: 4px;
}

@media (max-width: 768px) {
  .timeline-line {
    left: 10px;
  }
  
  .timeline-item {
    padding-left: 40px;
  }
  
  .timeline-marker {
    left: 2px;
  }
  
  .timeline-content {
    padding: 16px;
  }
}
</style>
