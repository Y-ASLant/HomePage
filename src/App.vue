<script setup>
import TypeJS from './components/TypeJS.vue';
import Hitokoto from './components/Hitokoto.vue';
import Copyrights from './components/Copyrights.vue';
import MyButton from './components/MyButton.vue';
import AboutModal from './components/AboutModal.vue';
</script>

<script>
export default {
  data() {
    return {
      showAboutModal: false,
      showTooltip: false,
      isMobile: false
    };
  },
  mounted() {
    this.checkIfMobile();
    this.setRandomBackgroundTheme();
    window.addEventListener('resize', this.checkIfMobile);
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.checkIfMobile);
  },
  methods: {
    handleShowAbout() {
      this.showAboutModal = true;
    },
    handleCloseAbout() {
      this.showAboutModal = false;
    },
    handleMouseEnter() {
      if (!this.isMobile) {
        this.showTooltip = true;
      }
    },
    handleMouseLeave() {
      if (!this.isMobile) {
        this.showTooltip = false;
      }
    },
    setRandomBackgroundTheme() {
      const themes = [
        {
          name: 'ocean',
          base: '#0b1320',
          accent1: 'rgba(87, 186, 255, 0.52)',
          accent2: 'rgba(78, 255, 214, 0.34)',
          accent3: 'rgba(121, 154, 255, 0.26)',
          from: '#0c1b3a',
          mid: '#1a3565',
          to: '#0b1320'
        },
        {
          name: 'sunset',
          base: '#1a1016',
          accent1: 'rgba(255, 136, 72, 0.55)',
          accent2: 'rgba(255, 83, 145, 0.36)',
          accent3: 'rgba(255, 208, 92, 0.24)',
          from: '#2a1422',
          mid: '#4d2340',
          to: '#1a1016'
        },
        {
          name: 'aurora',
          base: '#0d1418',
          accent1: 'rgba(118, 255, 174, 0.42)',
          accent2: 'rgba(126, 138, 255, 0.34)',
          accent3: 'rgba(114, 246, 255, 0.28)',
          from: '#0f2027',
          mid: '#1f3b49',
          to: '#0d1418'
        },
        {
          name: 'forest',
          base: '#0f1711',
          accent1: 'rgba(116, 235, 143, 0.44)',
          accent2: 'rgba(79, 186, 128, 0.3)',
          accent3: 'rgba(240, 255, 145, 0.22)',
          from: '#152419',
          mid: '#27422e',
          to: '#0f1711'
        },
        {
          name: 'graphite',
          base: '#111317',
          accent1: 'rgba(171, 191, 255, 0.34)',
          accent2: 'rgba(255, 179, 112, 0.24)',
          accent3: 'rgba(218, 222, 240, 0.16)',
          from: '#1a1f28',
          mid: '#2a3242',
          to: '#111317'
        }
      ];

      const selectedTheme = themes[Math.floor(Math.random() * themes.length)];
      const root = document.documentElement;

      root.style.setProperty('--bg-base', selectedTheme.base);
      root.style.setProperty('--bg-accent-1', selectedTheme.accent1);
      root.style.setProperty('--bg-accent-2', selectedTheme.accent2);
      root.style.setProperty('--bg-accent-3', selectedTheme.accent3);
      root.style.setProperty('--bg-gradient-from', selectedTheme.from);
      root.style.setProperty('--bg-gradient-mid', selectedTheme.mid);
      root.style.setProperty('--bg-gradient-to', selectedTheme.to);
      root.setAttribute('data-bg-theme', selectedTheme.name);
    },
    checkIfMobile() {
      this.isMobile = window.matchMedia('(max-width: 768px)').matches;
    }
  }
};
</script>

<template>
  <div id="main">
    <div class="center-flexbox">
      <div class="meBox">
        <div class="avatar-container">
          <button
            type="button"
            class="meBox-headPhoto"
            aria-label="打开关于面板"
            @click="handleShowAbout"
            @mouseenter="handleMouseEnter"
            @mouseleave="handleMouseLeave"
            @focus="handleMouseEnter"
            @blur="handleMouseLeave"
          ></button>
          <transition name="tooltip">
            <div v-if="showTooltip && !isMobile" class="tooltip">
              <div class="tooltip-content">
                <iconify-icon
                  icon="fluent:person-info-24-regular"
                  class="tooltip-icon"
                ></iconify-icon>
                <span>关于</span>
              </div>
              <div class="tooltip-arrow"></div>
            </div>
          </transition>
        </div>
        <TypeJS />
        <div class="meBox-line"></div>
        <div class="meBox-main">
          <Hitokoto />
        </div>
        <MyButton />
      </div>
    </div>
  </div>
  <Copyrights />

  <!-- 关于面板悬浮窗 -->
  <AboutModal :visible="showAboutModal" @close="handleCloseAbout" />
</template>

<style scoped>
.meBox {
  min-width: 32rem;
  max-width: 38rem;
}

.avatar-container {
  position: relative;
  display: inline-block;
  margin: 0 auto;

  /* 禁用容器的选中效果 */
  -webkit-tap-highlight-color: transparent;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  user-select: none;
}

.meBox-headPhoto {
  width: 8rem;
  height: 8rem;
  padding: 0;
  border: none;
  background-color: transparent;
  background: url(/favicon.ico);
  background-size: cover;
  border-radius: 50%;
  margin: 0 auto;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);
  margin-bottom: 2rem;
  animation: glassReveal 0.72s ease-out both;
  animation-delay: 0.08s;
  transition: transform 0.67s ease;
  cursor: pointer;

  /* 禁用移动端点击选中效果 */
  -webkit-tap-highlight-color: transparent;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.meBox-headPhoto:hover {
  transform: rotate(360deg) scale(1.1);
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.35);
}

.meBox-headPhoto:focus-visible {
  outline: 2px solid rgba(255, 255, 255, 0.78);
  outline-offset: 4px;
}

/* Tooltip 样式 */
.tooltip {
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  margin-bottom: 12px;
  z-index: 100;
}

.tooltip-content {
  background-color: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  color: white;
  padding: 8px 16px;
  border-radius: 8px;
  font-size: 0.9rem;
  white-space: nowrap;
  display: flex;
  align-items: center;
  gap: 6px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.tooltip-icon {
  font-size: 1.1em;
  color: #fff;
}

.tooltip-arrow {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 0;
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-top: 6px solid rgba(0, 0, 0, 0.85);
}

/* Tooltip 动画 */
.tooltip-enter-active,
.tooltip-leave-active {
  transition: all 0.2s ease;
}

.tooltip-enter-from,
.tooltip-leave-to {
  opacity: 0;
  transform: translateX(-50%) translateY(5px);
}

.tooltip-enter-to,
.tooltip-leave-from {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

.meBox-main {
  padding: 2rem;
  backdrop-filter: blur(15px);
  -webkit-backdrop-filter: blur(15px);
  border-radius: 10px;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: #00000043;
  animation: glassReveal 0.76s ease-out both;
  animation-delay: 0.28s;
  transition: transform 0.3s ease;
}

.meBox-main:hover {
  transform: scale(1.01);
}

.meBox-line {
  width: 120px;
  height: 0.5px;
  background-color: #fffffff3;
  margin: 25px auto 35px auto;
  animation: glassReveal 0.7s ease-out both;
  animation-delay: 0.2s;
  transition: transform 0.67s ease;
}

@media screen and (max-width: 560px) {
  .meBox {
    min-width: 100%;
  }

  .meBox-main {
    box-sizing: border-box;
  }

  /* 移动端完全隐藏tooltip */
  .tooltip {
    display: none !important;
  }
}
</style>
