<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import ParticleBackground from './components/ParticleBackground.vue'
import CustomCursor from './components/CustomCursor.vue'
import Navigation from './components/Navigation.vue'
import HeroSection from './components/HeroSection.vue'
import AboutSection from './components/AboutSection.vue'
import SkillsSection from './components/SkillsSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import ExperienceSection from './components/ExperienceSection.vue'
import ContactSection from './components/ContactSection.vue'
import LoadingScreen from './components/LoadingScreen.vue'

gsap.registerPlugin(ScrollTrigger)

const isLoading = ref(true)
const showContent = ref(false)

const handleLoadingComplete = () => {
  isLoading.value = false
  setTimeout(() => {
    showContent.value = true
  }, 100)
}

onMounted(() => {
  document.body.style.overflow = isLoading.value ? 'hidden' : 'auto'
})

onUnmounted(() => {
  ScrollTrigger.getAll().forEach(trigger => trigger.kill())
})
</script>

<template>
  <LoadingScreen v-if="isLoading" @complete="handleLoadingComplete" />
  
  <div class="app" :class="{ visible: showContent }">
    <CustomCursor />
    <ParticleBackground />
    <Navigation />
    
    <main>
      <HeroSection />
      <AboutSection />
      <SkillsSection />
      <ProjectsSection />
      <ExperienceSection />
      <ContactSection />
    </main>
    
    <footer class="footer">
      <div class="container">
        <p>&copy; 2024 Developer Portfolio. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.app {
  opacity: 0;
  transition: opacity 0.8s ease;
}

.app.visible {
  opacity: 1;
}

.footer {
  padding: 40px 0;
  background: var(--bg-secondary);
  text-align: center;
  color: var(--text-muted);
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}

.footer p {
  font-size: 0.875rem;
}
</style>
