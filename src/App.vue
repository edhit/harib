<script setup>
import { ref } from 'vue'

// 1. Список Джузов
const links = ref([
  { name: 'الجزء 1', file: 'Garibu_Kuran_part_1.html', description: 'Al-Fatihah — Al-Baqarah' },
  { name: 'الجزء 2', file: 'Garibu_Kuran_part_2.html', description: 'Al-Baqarah (continuation)' },
  { name: 'الجزء 3', file: 'Garibu_Kuran_part_3.html', description: 'Al-Baqarah — Al-Imran' }
])

const activeTest = ref(null)

const openTest = (file) => {
  if (window.Telegram?.WebApp) {
    window.Telegram.WebApp.HapticFeedback.impactOccurred('light')
  }
  activeTest.value = file
}

const closeTest = () => {
  activeTest.value = null
}
</script>

<template>
  <div class="app-container">
    <!-- СИНИЙ NAVIGATION BAR (TOOLBAR) -->
    <nav class="navigation-bar">
      <div class="nav-content">
        <span class="nav-logo">Garibu Kuran</span>
          <div class="nav-actions">
          <span class="tg-logo-link">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 2C6.48 2 2 6.48 2 12C2 17.52 6.48 22 12 22C17.52 22 22 17.52 22 12C22 6.48 17.52 2 12 2ZM16.64 8.8C16.49 10.38 15.84 14.18 15.51 15.96C15.37 16.71 15.09 16.96 14.83 16.99C14.25 17.04 13.81 16.61 13.25 16.24C12.37 15.66 11.87 15.3 11.02 14.74C10.03 14.09 10.67 13.73 11.24 13.15C11.39 12.99 13.94 10.68 13.99 10.46C14 10.43 14.01 10.33 13.95 10.28C13.89 10.23 13.8 10.25 13.73 10.26C13.63 10.28 12.13 11.28 9.22 13.24C8.8 13.54 8.41 13.68 8.07 13.67C7.7 13.66 6.98 13.46 6.45 13.29C5.79 13.08 5.27 12.97 5.32 12.61C5.34 12.42 5.6 12.23 6.08 12.03C9.07 10.73 11.06 9.87 13.06 9.04C15.82 7.9 16.4 7.7 16.77 7.7C16.85 7.7 17.04 7.72 17.16 7.82C17.26 7.9 17.29 8.02 17.3 8.1C17.31 8.24 17.3 8.52 16.64 8.8Z" fill="white"/>
            </svg>
          </span>
        </div>
      </div>
    </nav>

    <!-- ОСНОВНОЙ КОНТЕНТ -->
    <main class="home-screen">
      <header class="content-header">
        <h1 class="main-title">قائمة الأجزاء</h1>
        <div class="hadith-container">
          <p class="arabic-hadith">«خَيْرُكُمْ مَنْ تَعَلَّمَ الْقُرْآنَ وَعَلَّمَهُ»</p>
          <p class="main-subtitle">«Лучший из вас тот, кто изучил Коран и обучил ему других»</p>
        </div>
      </header>

      <div class="list">
        <div 
          v-for="link in links" 
          :key="link.file" 
          @click="openTest(link.file)" 
          class="item"
        >
          <div class="item-info">
            <span class="item-name">{{ link.name }}</span>
          </div>
          <div class="arrow-wrapper">
            <span class="arrow">→</span>
          </div>
        </div>
      </div>
      
      <!-- <p class="coming-soon">More coming soon!</p> -->
    </main>

    <!-- ОКНО ПРОСМОТРА (iOS Slide Animation) -->
    <transition name="ios-slide">
      <div v-if="activeTest" class="test-overlay">
        <div class="header-nav">
          <button @click="closeTest" class="back-button">رجوع</button>
          <span class="view-title">الاختبار</span>
          <div style="width: 60px"></div>
        </div>
        <iframe :src="`/tests/${activeTest}`"></iframe>
      </div>
    </transition>
  </div>
</template>

<style>
/* ОСНОВНЫЕ СТИЛИ И ШРИФТЫ */
body, html {
  margin: 0; padding: 0;
  overflow: hidden;
  font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "Segoe UI", Roboto, sans-serif;
  background-color: #f2f2f7;
  -webkit-font-smoothing: antialiased;
}

.app-container {
  position: relative;
  height: 100vh;
  display: flex;
  flex-direction: column;
}

/* NAVIGATION BAR (Синий тулбар сверху) */
.navigation-bar {
  background-color: #248bcf;
  height: 50px;
  display: flex;
  align-items: center;
  padding: 0 16px;
  color: white;
  z-index: 10;
}

.nav-content {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-logo {
  font-weight: 700;
  font-size: 17px;
  letter-spacing: -0.2px;
}

/* ЗАГОЛОВОК (Стиль из 1-го варианта) */
.content-header {
  padding: 24px 20px 10px;
}

.main-title {
  font-size: 34px;
  font-weight: 800;
  color: #000;
  margin: 0;
  letter-spacing: -1px;
}

.main-subtitle {
  font-size: 15px;
  color: #8e8e93;
  margin: 4px 0 0;
  font-weight: 400;
}

.nav-actions {
  display: flex;
  align-items: center;
}

.tg-logo-link {
  display: flex;
  align-items: center;
  justify-content: center;
  transition: opacity 0.2s ease, transform 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.tg-logo-link:active {
  opacity: 0.7;
  transform: scale(0.9);
}

/* Удалите старый стиль .circle-btn, он больше не нужен */

/* СПИСОК КАРТОЧЕК */
.home-screen {
  flex-grow: 1;
  overflow-y: auto;
  padding-bottom: 30px;
}

/* Обновленный список карточек */
.list {
  padding: 16px; /* Внешний отступ списка от краев экрана */
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.item {
  background: white;
  /* Уменьшаем padding-left до 12px-15px, чтобы текст был ближе к краю */
  padding: 18px 15px; 
  border-radius: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.04);
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  -webkit-tap-highlight-color: transparent;
}

.item-info {
  display: flex;
  flex-direction: column;
  gap: 3px;
  text-align: left; /* Гарантируем выравнивание по левому краю */
  flex-grow: 1;    /* Позволяет тексту занимать все свободное место слева */
}

.item-name {
  font-size: 19px;
  font-weight: 700;
  color: #000;
  margin: 0;       /* Убираем возможные внешние отступы */
}

.item-desc {
  font-size: 14px;
  color: #8e8e93;
  margin: 0;       /* Убираем возможные внешние отступы */
}


.main-title {
  font-size: 44px; /* Арабский шрифт требует большего размера */
  font-weight: 800;
  color: #000;
  margin: 0;
  direction: rtl; /* Справа налево */
}

.hadith-container {
  margin-top: 10px;
  border-left: 3px solid #248bcf; /* Синяя полоска слева как у цитаты */
  padding-left: 12px;
}

.arabic-hadith {
  font-family: "Amiri", "Times New Roman", serif;
  font-size: 20px;
  font-weight: 600;
  color: #248bcf;
  direction: rtl;
  margin: 25px 0 0;
}

.main-subtitle {
  font-size: 14px;
  color: #8e8e93;
  margin: 4px 0 0;
  font-style: italic;
  line-height: 1.4;
}

/* Опционально: если заголовок "Список частей" тоже нужно левее */
.content-header {
  padding: 24px 15px 10px; /* Уменьшили боковой отступ с 20 до 15 */
}

.arrow-wrapper {
  background: #f2f2f7;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.arrow {
  color: #248bcf;
  font-size: 18px;
  font-weight: bold;
}

.coming-soon {
  text-align: center;
  color: #c7c7cc;
  font-size: 14px;
  margin-top: 20px;
}

/* OVERLAY С ТЕСТОМ */
.test-overlay {
  position: fixed;
  top: 0; left: 0; width: 100%; height: 100%;
  background: white;
  z-index: 1000;
  display: flex;
  flex-direction: column;
}

.header-nav {
  height: 54px;
  background: white;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 0.5px solid #d1d1d6;
  padding: 0 12px;
}

.back-button {
  background: none; border: none;
  color: #248bcf; font-size: 17px; font-weight: 600;
  cursor: pointer;
  padding: 8px 0;
}

.view-title {
  font-weight: 700;
  font-size: 17px;
}

iframe {
  flex-grow: 1;
  border: none;
}

/* АНИМАЦИЯ IPHONE */
.ios-slide-enter-active, .ios-slide-leave-active {
  transition: transform 0.35s cubic-bezier(0.3, 0.8, 0.3, 1);
}
.ios-slide-enter-from, .ios-slide-leave-to {
  transform: translateX(100%);
}
</style>