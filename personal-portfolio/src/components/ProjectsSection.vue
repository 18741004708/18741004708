<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const sectionRef = ref(null)
const activeFilter = ref('all')

const filters = ['all', 'web', 'mobile', 'backend']

const projects = [
  {
    id: 1,
    title: 'E-Commerce Platform',
    description: 'A full-featured online shopping platform with real-time inventory, payment processing, and admin dashboard.',
    image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=modern%20e-commerce%20website%20dashboard%20interface%20clean%20design&image_size=landscape_16_9',
    tags: ['Vue.js', 'Node.js', 'MongoDB', 'Stripe'],
    category: 'web',
    link: '#',
    github: '#'
  },
  {
    id: 2,
    title: 'Task Management App',
    description: 'Collaborative task management application with real-time updates, drag-and-drop, and team features.',
    image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=task%20management%20app%20interface%20kanban%20board%20modern&image_size=landscape_16_9',
    tags: ['React', 'Firebase', 'Material-UI'],
    category: 'web',
    link: '#',
    github: '#'
  },
  {
    id: 3,
    title: 'Fitness Tracking Mobile App',
    description: 'Cross-platform mobile app for tracking workouts, nutrition, and health metrics with AI recommendations.',
    image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=fitness%20tracking%20mobile%20app%20interface%20dark%20theme&image_size=landscape_16_9',
    tags: ['React Native', 'Node.js', 'PostgreSQL'],
    category: 'mobile',
    link: '#',
    github: '#'
  },
  {
    id: 4,
    title: 'API Gateway Service',
    description: 'High-performance API gateway with rate limiting, authentication, and analytics for microservices.',
    image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=api%20architecture%20diagram%20microservices%20technical&image_size=landscape_16_9',
    tags: ['Node.js', 'Redis', 'Docker', 'Kubernetes'],
    category: 'backend',
    link: '#',
    github: '#'
  },
  {
    id: 5,
    title: 'Real-time Chat Application',
    description: 'Scalable chat platform with end-to-end encryption, file sharing, and video calling features.',
    image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=chat%20application%20interface%20modern%20messaging%20app&image_size=landscape_16_9',
    tags: ['Vue.js', 'Socket.io', 'WebRTC'],
    category: 'web',
    link: '#',
    github: '#'
  },
  {
    id: 6,
    title: 'Data Analytics Dashboard',
    description: 'Interactive dashboard for visualizing business metrics with real-time data streaming and custom reports.',
    image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=data%20analytics%20dashboard%20charts%20graphs%20modern%20ui&image_size=landscape_16_9',
    tags: ['React', 'D3.js', 'Python', 'FastAPI'],
    category: 'web',
    link: '#',
    github: '#'
  }
]

const filteredProjects = ref(projects)

const filterProjects = (filter) => {
  activeFilter.value = filter
  if (filter === 'all') {
    filteredProjects.value = projects
  } else {
    filteredProjects.value = projects.filter(p => p.category === filter)
  }
}

const handleCardHover = (e, isEntering) => {
  const card = e.currentTarget
  const rect = card.getBoundingClientRect()
  const x = e.clientX - rect.left
  const y = e.clientY - rect.top
  
  const centerX = rect.width / 2
  const centerY = rect.height / 2
  
  const rotateX = (y - centerY) / 10
  const rotateY = (centerX - x) / 10
  
  if (isEntering) {
    gsap.to(card, {
      rotateX: -rotateX,
      rotateY: rotateY,
      duration: 0.3,
      ease: 'power2.out'
    })
  } else {
    gsap.to(card, {
      rotateX: 0,
      rotateY: 0,
      duration: 0.5,
      ease: 'elastic.out(1, 0.5)'
    })
  }
}

onMounted(() => {
  gsap.fromTo('.project-card',
    { opacity: 0, y: 50 },
    { 
      opacity: 1, 
      y: 0, 
      duration: 0.8,
      stagger: 0.15,
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
  <section id="projects" class="projects-section section" ref="sectionRef">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">Featured Projects</h2>
        <p class="section-subtitle">Some of my recent work</p>
      </div>
      
      <div class="filter-buttons">
        <button 
          v-for="filter in filters"
          :key="filter"
          class="filter-btn"
          :class="{ active: activeFilter === filter }"
          @click="filterProjects(filter)"
        >
          {{ filter.charAt(0).toUpperCase() + filter.slice(1) }}
        </button>
      </div>
      
      <div class="projects-grid">
        <div 
          v-for="project in filteredProjects"
          :key="project.id"
          class="project-card"
          @mousemove="handleCardHover($event, true)"
          @mouseleave="handleCardHover($event, false)"
        >
          <div class="card-image">
            <img :src="project.image" :alt="project.title" loading="lazy" />
            <div class="card-overlay">
              <div class="overlay-content">
                <a :href="project.link" class="overlay-btn" target="_blank">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
                    <polyline points="15 3 21 3 21 9"/>
                    <line x1="10" y1="14" x2="21" y2="3"/>
                  </svg>
                  Live Demo
                </a>
                <a :href="project.github" class="overlay-btn" target="_blank">
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0 0 24 12c0-6.63-5.37-12-12-12z"/>
                  </svg>
                  Code
                </a>
              </div>
            </div>
          </div>
          
          <div class="card-content">
            <h3 class="card-title">{{ project.title }}</h3>
            <p class="card-description">{{ project.description }}</p>
            <div class="card-tags">
              <span v-for="tag in project.tags" :key="tag" class="tag">{{ tag }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.projects-section {
  background: var(--bg-secondary);
  position: relative;
}

.projects-section::before {
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
  margin-bottom: 40px;
}

.filter-buttons {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-bottom: 50px;
  flex-wrap: wrap;
}

.filter-btn {
  padding: 10px 24px;
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--text-secondary);
  background: var(--bg-tertiary);
  border-radius: 20px;
  transition: all 0.3s ease;
  border: 1px solid transparent;
}

.filter-btn:hover {
  color: var(--text-primary);
  border-color: rgba(255, 255, 255, 0.1);
}

.filter-btn.active {
  background: var(--gradient-primary);
  color: var(--bg-primary);
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 30px;
}

.project-card {
  background: var(--bg-primary);
  border-radius: var(--border-radius);
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.3s ease;
  transform-style: preserve-3d;
  perspective: 1000px;
}

.project-card:hover {
  box-shadow: var(--shadow-glow);
}

.card-image {
  position: relative;
  height: 200px;
  overflow: hidden;
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.project-card:hover .card-image img {
  transform: scale(1.1);
}

.card-overlay {
  position: absolute;
  inset: 0;
  background: rgba(10, 10, 15, 0.9);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.project-card:hover .card-overlay {
  opacity: 1;
}

.overlay-content {
  display: flex;
  gap: 16px;
}

.overlay-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 20px;
  background: var(--gradient-primary);
  color: var(--bg-primary);
  border-radius: 8px;
  font-size: 0.875rem;
  font-weight: 600;
  transition: all 0.3s ease;
}

.overlay-btn:hover {
  transform: scale(1.05);
}

.overlay-btn svg {
  width: 18px;
  height: 18px;
}

.card-content {
  padding: 24px;
}

.card-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 12px;
  color: var(--text-primary);
}

.card-description {
  font-size: 0.875rem;
  color: var(--text-secondary);
  line-height: 1.6;
  margin-bottom: 16px;
}

.card-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.tag {
  padding: 4px 12px;
  background: var(--bg-tertiary);
  border-radius: 4px;
  font-size: 0.75rem;
  color: var(--accent-cyan);
  font-family: 'JetBrains Mono', monospace;
}

@media (max-width: 768px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
}
</style>
