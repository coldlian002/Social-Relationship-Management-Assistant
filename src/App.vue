<script setup lang="ts">
import { ref, shallowRef } from 'vue'
import Dashboard from './components/Dashboard.vue'
import ContactManagement from './components/ContactManagement.vue'
import EventReminders from './components/EventReminders.vue'
import GiftRecommendation from './components/GiftRecommendation.vue'

const currentView = ref('dashboard')
const currentComponent = shallowRef(Dashboard)

function changeView(view: string) {
  currentView.value = view
  switch (view) {
    case 'dashboard':
      currentComponent.value = Dashboard
      break
    case 'contacts':
      currentComponent.value = ContactManagement
      break
    case 'events':
      currentComponent.value = EventReminders
      break
    case 'gifts':
      currentComponent.value = GiftRecommendation
      break
  }
}
</script>

<template>
  <div class="app-container">
    <header class="app-header glass-effect">
      <h1>社交关系管理助手</h1>
      <nav class="nav-menu">
        <button 
          v-for="(label, key) in {
            dashboard: '仪表盘',
            contacts: '联系人管理',
            events: '事件提醒',
            gifts: '礼物推荐'
          }"
          :key="key"
          @click="changeView(key)"
          :class="['nav-button', { active: currentView === key }]"
        >
          <span class="nav-icon" :class="key"></span>
          {{ label }}
        </button>
      </nav>
    </header>
    
    <main class="main-content">
      <component :is="currentComponent"></component>
    </main>
  </div>
</template>

<style scoped>
.app-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #f6f8fc 0%, #e9edf5 100%);
}

.app-header {
  padding: 1.5rem 2rem;
  margin-bottom: 2rem;
  border-radius: 0 0 24px 24px;
}

h1 {
  font-size: 1.75rem;
  font-weight: 700;
  color: var(--text-primary);
  margin: 0 0 1.5rem 0;
  letter-spacing: -0.02em;
}

.nav-menu {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
}

.nav-button {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.25rem;
  border: none;
  border-radius: 12px;
  background: transparent;
  color: var(--text-secondary);
  font-size: 0.95rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.nav-button:hover {
  background: rgba(37, 99, 235, 0.1);
  color: var(--primary);
}

.nav-button.active {
  background: var(--primary);
  color: white;
}

.nav-icon {
  width: 20px;
  height: 20px;
  display: inline-block;
  background-size: contain;
  opacity: 0.7;
}

.main-content {
  padding: 0 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .app-header {
    padding: 1rem;
  }
  
  .nav-menu {
    flex-direction: column;
  }
  
  .nav-button {
    width: 100%;
    justify-content: center;
  }
  
  .main-content {
    padding: 0 1rem;
  }
}
</style>