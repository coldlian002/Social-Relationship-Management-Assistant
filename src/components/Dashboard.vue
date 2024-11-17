<script setup lang="ts">
import { ref, computed } from 'vue'

interface Event {
  id: number
  name: string
  date: string
  type: string
  daysUntil: number
}

interface Fortune {
  date: string
  horoscope: string
  luck: number
  color: string
  numbers: string
  advice: string
}

const upcomingEvents = ref<Event[]>([
  { id: 1, name: '妈妈生日', date: '2024-03-15', type: '生日', daysUntil: 7 },
  { id: 2, name: '李四结婚纪念日', date: '2024-03-20', type: '纪念日', daysUntil: 12 },
  { id: 3, name: '张三升职宴会', date: '2024-03-10', type: '社交活动', daysUntil: 2 }
])

const dailyFortune = ref<Fortune>({
  date: new Date().toLocaleDateString('zh-CN'),
  horoscope: '今天的你充满魅力与活力，是展现自我的好时机',
  luck: 89,
  color: '粉色',
  numbers: '3, 7, 9',
  advice: '今天适合社交活动，与朋友分享快乐时光'
})

const urgentEvents = computed(() => {
  return upcomingEvents.value
    .filter(event => event.daysUntil <= 7)
    .sort((a, b) => a.daysUntil - b.daysUntil)
})

const luckLevel = computed(() => {
  if (dailyFortune.value.luck >= 90) return '超级幸运'
  if (dailyFortune.value.luck >= 80) return '幸运'
  if (dailyFortune.value.luck >= 70) return '较好'
  return '普通'
})
</script>

<template>
  <div class="dashboard">
    <div class="dashboard-grid">
      <!-- 紧急事项卡片 -->
      <div class="dashboard-card glass-effect">
        <div class="card-header">
          <h3>⚠️ 紧急事项</h3>
          <span class="badge">{{ urgentEvents.length }}</span>
        </div>
        <div class="card-content">
          <ul v-if="urgentEvents.length">
            <li v-for="event in urgentEvents" :key="event.id" class="urgent-item">
              <div class="event-info">
                <span class="event-name">{{ event.name }}</span>
                <span class="event-type">{{ event.type }}</span>
              </div>
              <span class="event-days">还有 {{ event.daysUntil }} 天</span>
            </li>
          </ul>
          <p v-else class="empty-message">近期没有紧急事项</p>
        </div>
      </div>

      <!-- 每日运势卡片 -->
      <div class="dashboard-card glass-effect fortune-card">
        <div class="card-header">
          <h3>✨ 今日运势</h3>
          <span class="date">{{ dailyFortune.date }}</span>
        </div>
        <div class="card-content">
          <div class="fortune-content">
            <div class="fortune-main">
              <div class="fortune-text">{{ dailyFortune.horoscope }}</div>
              <div class="fortune-luck">
                <div class="luck-meter">
                  <div class="luck-fill" :style="{ width: dailyFortune.luck + '%' }"></div>
                </div>
                <div class="luck-label">幸运指数: {{ luckLevel }}</div>
              </div>
            </div>
            <div class="fortune-details">
              <div class="fortune-item">
                <span class="label">幸运色:</span>
                <span class="value">{{ dailyFortune.color }}</span>
              </div>
              <div class="fortune-item">
                <span class="label">幸运数字:</span>
                <span class="value">{{ dailyFortune.numbers }}</span>
              </div>
            </div>
            <div class="fortune-advice">
              <span class="advice-icon">💫</span>
              <span class="advice-text">{{ dailyFortune.advice }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- 统计概览 -->
      <div class="dashboard-card glass-effect">
        <div class="card-header">
          <h3>📊 统计概览</h3>
        </div>
        <div class="stat-grid">
          <div class="stat-item">
            <div class="stat-value">{{ upcomingEvents.length }}</div>
            <div class="stat-label">近期事项</div>
          </div>
          <div class="stat-item">
            <div class="stat-value">{{ urgentEvents.length }}</div>
            <div class="stat-label">紧急事项</div>
          </div>
          <div class="stat-item">
            <div class="stat-value">{{ dailyFortune.luck }}%</div>
            <div class="stat-label">今日幸运值</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.dashboard {
  padding: 1rem;
}

.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 1.5rem;
  margin-top: 1rem;
}

.dashboard-card {
  border-radius: 20px;
  overflow: hidden;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.25rem 1.5rem;
  border-bottom: 1px solid var(--border);
}

h3 {
  margin: 0;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--text-primary);
}

.badge {
  background: var(--primary-light);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  font-size: 0.875rem;
  font-weight: 500;
}

.card-content {
  padding: 1.25rem 1.5rem;
}

/* 紧急事项样式 */
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.urgent-item {
  padding: 1rem;
  border-radius: 12px;
  background: rgba(244, 63, 94, 0.1);
  margin-bottom: 0.75rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: transform 0.2s ease;
}

.urgent-item:hover {
  transform: translateY(-2px);
}

.event-info {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.event-name {
  font-weight: 500;
  color: var(--text-primary);
}

.event-type {
  font-size: 0.875rem;
  color: var(--text-secondary);
}

.event-days {
  color: var(--accent);
  font-weight: 600;
  font-size: 0.875rem;
}

/* 运势卡片样式 */
.fortune-card {
  background: linear-gradient(135deg, rgba(255,255,255,0.9), rgba(255,255,255,0.7));
}

.date {
  font-size: 0.875rem;
  color: var(--text-secondary);
}

.fortune-content {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.fortune-main {
  text-align: center;
}

.fortune-text {
  font-size: 1.1rem;
  color: var(--text-primary);
  margin-bottom: 1.5rem;
  line-height: 1.6;
}

.fortune-luck {
  margin-top: 1rem;
}

.luck-meter {
  height: 8px;
  background: rgba(203, 213, 225, 0.3);
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 0.5rem;
}

.luck-fill {
  height: 100%;
  background: linear-gradient(90deg, #60a5fa, #3b82f6);
  border-radius: 4px;
  transition: width 1s ease-in-out;
}

.luck-label {
  font-size: 0.875rem;
  color: var(--text-secondary);
}

.fortune-details {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 12px;
}

.fortune-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.label {
  font-size: 0.875rem;
  color: var(--text-secondary);
}

.value {
  font-weight: 600;
  color: var(--primary);
}

.fortune-advice {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 12px;
}

.advice-icon {
  font-size: 1.25rem;
}

.advice-text {
  font-size: 0.95rem;
  color: var(--text-primary);
  line-height: 1.5;
}

/* 统计概览样式 */
.stat-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  padding: 1rem;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  transition: transform 0.2s ease;
}

.stat-item:hover {
  transform: translateY(-2px);
}

.stat-value {
  font-size: 1.75rem;
  font-weight: 700;
  color: var(--primary);
  line-height: 1;
  margin-bottom: 0.5rem;
}

.stat-label {
  font-size: 0.875rem;
  color: var(--text-secondary);
  text-align: center;
}

.empty-message {
  text-align: center;
  color: var(--text-secondary);
  padding: 2rem;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 12px;
  font-size: 0.95rem;
}

@media (max-width: 768px) {
  .dashboard-grid {
    grid-template-columns: 1fr;
  }
  
  .stat-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  .stat-grid {
    grid-template-columns: 1fr;
  }
  
  .card-header {
    flex-direction: column;
    gap: 0.5rem;
    text-align: center;
  }
  
  .fortune-details {
    grid-template-columns: 1fr;
  }
}
</style>