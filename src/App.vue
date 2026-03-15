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
      isMobile: false,
      backgroundImages: ['0.webp', '1.webp', '2.webp'],
      currentBackground: ''
    };
  },
  mounted() {
    this.checkIfMobile();
    this.setRandomBackground();
    window.addEventListener('resize', this.checkIfMobile);
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.checkIfMobile);
  },
  methods: {
    handleAnimationEnd(event) {
      event.target.style.transition = 'transform 0.3s ease';
    },
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
    checkIfMobile() {
      this.isMobile = window.innerWidth <= 768 || 'ontouchstart' in window;
    },
    setRandomBackground() {
      const randomIndex = Math.floor(Math.random() * this.backgroundImages.length);
      this.currentBackground = this.backgroundImages[randomIndex];
      document.body.style.backgroundImage = `url(/background/${this.currentBackground})`;
    }
  }
};
</script>

<template>
  <div id="main">
    <div class="center-flexbox">
      <div class="meBox">
        <div class="avatar-container">
          <div
            class="meBox-headPhoto"
            @click="handleShowAbout"
            @mouseenter="handleMouseEnter"
            @mouseleave="handleMouseLeave"
          ></div>
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
  background: url(/favicon.ico);
  background-size: cover;
  border-radius: 50%;
  margin: 0 auto;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);
  margin-bottom: 2rem;
  animation: floatUp 1s ease-out;
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
  outline: none;
}

.meBox-headPhoto:hover {
  transform: rotate(360deg) scale(1.1);
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.35);
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
  animation: floatUp 1s ease-out;
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
  animation: floatUp 1s ease-out;
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
