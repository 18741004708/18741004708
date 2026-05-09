<script setup>
import { useI18n } from 'vue-i18n'

const { locale, availableLocales } = useI18n()

const toggleLanguage = () => {
  const newLocale = locale.value === 'zh' ? 'en' : 'zh'
  locale.value = newLocale
  localStorage.setItem('language', newLocale)
}
</script>

<template>
  <div class="language-switch">
    <button 
      class="lang-btn"
      @click="toggleLanguage"
    >
      <span class="lang-text">{{ locale === 'zh' ? 'EN' : '中文' }}</span>
    </button>
  </div>
</template>

<style scoped>
.language-switch {
  position: fixed;
  top: 100px;
  right: 24px;
  z-index: 99;
}

.lang-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  background: var(--bg-secondary);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
}

.lang-btn:hover {
  border-color: var(--accent-cyan);
  transform: scale(1.1);
  box-shadow: var(--shadow-glow);
}

.lang-text {
  font-size: 0.875rem;
  font-weight: 600;
  color: var(--text-primary);
  font-family: 'JetBrains Mono', monospace;
}

@media (max-width: 768px) {
  .language-switch {
    top: auto;
    bottom: 24px;
    right: 24px;
  }
}
</style>
