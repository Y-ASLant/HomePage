<script>
export default {
  name: 'AboutModal',
  props: {
    visible: {
      type: Boolean,
      default: false
    }
  },
  emits: ['close'],
  data() {
    return {
      showCopyTooltip: false,
      copySuccess: false
    };
  },
  methods: {
    closeModal() {
      this.$emit('close');
    },
    handleOverlayClick(event) {
      if (event.target === event.currentTarget) {
        this.closeModal();
      }
    },
    async copyEmail() {
      try {
        await navigator.clipboard.writeText('aslant0607@gmail.com');
        this.copySuccess = true;
        this.showCopyTooltip = false;
        setTimeout(() => {
          this.copySuccess = false;
        }, 2000);
      } catch (err) {
        console.error('复制失败:', err);
        this.fallbackCopyTextToClipboard('aslant0607@gmail.com');
      }
    },
    fallbackCopyTextToClipboard(text) {
      const textArea = document.createElement('textarea');
      textArea.value = text;
      textArea.style.position = 'fixed';
      textArea.style.left = '-999999px';
      textArea.style.top = '-999999px';
      document.body.appendChild(textArea);
      textArea.focus();
      textArea.select();
      try {
        document.execCommand('copy');
        this.copySuccess = true;
        this.showCopyTooltip = false;
        setTimeout(() => {
          this.copySuccess = false;
        }, 2000);
      } catch (err) {
        console.error('降级复制也失败了:', err);
      }
      document.body.removeChild(textArea);
    }
  }
};
</script>

<template>
  <transition name="modal">
    <div v-if="visible" class="modal-overlay" @click="handleOverlayClick">
      <div class="modal-container">
        <div class="modal-header">
          <h2>关于</h2>
          <button class="close-btn" @click="closeModal">
            <iconify-icon icon="fluent:dismiss-24-regular" height="1.2em" width="1.2em"></iconify-icon>
          </button>
        </div>
        
        <div class="modal-content">
          <div class="about-section">
            <div class="avatar-section">
              <div class="avatar"></div>
              <h3>ASLant</h3>
              <p class="subtitle">嵌入式开发 & 全栈开发 & 技术爱好者</p>
            </div>
            
            <div class="info-section">
              <div class="info-item">
                <iconify-icon icon="logos:google-maps" class="info-icon" width="20" height="20"></iconify-icon>
                <span>中国.山东.济南</span>
              </div>
              <div class="info-item">
                <iconify-icon icon="logos:google-gmail" class="info-icon" width="20" height="20"></iconify-icon>
                <span class="email-text email-container" 
                      @mouseenter="showCopyTooltip = true"
                      @mouseleave="showCopyTooltip = false"
                      @click="copyEmail">aslant0607@gmail.com
                  <transition name="email-tooltip">
                    <div v-if="showCopyTooltip && !copySuccess" class="email-tooltip">
                      <div class="tooltip-content">
                        <iconify-icon icon="fluent:copy-24-regular" class="tooltip-icon"></iconify-icon>
                        <span>点击复制</span>
                      </div>
                      <div class="tooltip-arrow"></div>
                    </div>
                  </transition>
                  <transition name="email-tooltip">
                    <div v-if="copySuccess" class="email-tooltip success">
                      <div class="tooltip-content">
                        <iconify-icon icon="fluent:checkmark-24-regular" class="tooltip-icon"></iconify-icon>
                        <span>已复制</span>
                      </div>
                      <div class="tooltip-arrow"></div>
                    </div>
                  </transition>
                </span>
              </div>
              <div class="info-item">
                <iconify-icon icon="logos:telegram" class="info-icon" width="20" height="20"></iconify-icon>
                <a href="https://t.me/ASLant7" target="_blank" class="tg-link">@ASLant7</a>
              </div>
              <div class="info-item">
                <iconify-icon icon="fluent-emoji-flat:penguin" class="info-icon" width="20" height="20"></iconify-icon>
                <span class="qq-text">2041438004</span>
              </div>
            </div>
            
            <div class="description-section">
              <h4>简介</h4>
              <p>
                热爱技术，热爱开源，热爱分享。
              </p>
            </div>
            
            <div class="skills-section">
              <h4>技能标签</h4>
              <div class="skills-tags">
                <span class="skill-tag">Python</span>
                <span class="skill-tag">Qt</span>
                <span class="skill-tag">Vue</span>
                <span class="skill-tag">Android</span>
                <span class="skill-tag">Linux</span>
                <span class="skill-tag">Docker</span>
                <span class="skill-tag">Go</span>
                <span class="skill-tag">C/C++</span>
                <span class="skill-tag">Yolo</span>
                <span class="skill-tag">OpenCV</span>
                <span class="skill-tag">Kotlin</span>
                <span class="skill-tag">ARM单片机</span>
                <span class="skill-tag">树莓派</span>
              </div>
            </div>
          </div>
        </div>
        
        <div class="modal-footer">
          <p class="copyright">© 2025 ASLant. All rights reserved.</p>
        </div>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-container {
  background-color: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-radius: 16px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
  max-width: 500px;
  width: 90%;
  max-height: 80vh;
  overflow-y: auto;
  position: relative;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 24px 16px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

.modal-header h2 {
  margin: 0;
  color: #333;
  font-size: 1.5rem;
  font-weight: 600;
}

.close-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  border-radius: 6px;
  color: #666;
  transition: all 0.2s ease;
}

.close-btn:hover {
  background-color: rgba(0, 0, 0, 0.1);
  color: #333;
}

.modal-content {
  padding: 24px;
}

.about-section {
  text-align: center;
}

.avatar-section {
  margin-bottom: 24px;
}

.avatar {
  width: 80px;
  height: 80px;
  background: url(/about.png);
  background-size: cover;
  border-radius: 50%;
  margin: 0 auto 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.avatar-section h3 {
  margin: 0 0 8px;
  color: #333;
  font-size: 1.4rem;
  font-weight: 600;
}

.subtitle {
  margin: 0 0 20px;
  color: #666;
  font-size: 0.95rem;
}

.info-section {
  margin-bottom: 24px;
  text-align: left;
}

.info-item {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
  color: #555;
  font-size: 0.9rem;
}

.email-container {
  position: relative;
}

.email-text {
  cursor: pointer;
  padding: 4px 8px;
  margin: -4px -8px;
  border-radius: 6px;
  transition: all 0.2s ease;
}

.email-text:hover {
  background-color: rgba(0, 0, 0, 0.05);
}

.tg-link {
  color: #555;
  text-decoration: none;
  padding: 4px 8px;
  margin: -4px -8px;
  border-radius: 6px;
  transition: all 0.2s ease;
}

.tg-link:hover {
  background-color: rgba(0, 0, 0, 0.05);
  color: #0088cc;
}

.qq-text {
  color: #555;
  padding: 4px 8px;
  margin: -4px -8px;
  border-radius: 6px;
  transition: all 0.2s ease;
  cursor: pointer;
}

.qq-text:hover {
  background-color: rgba(0, 0, 0, 0.05);
  color: #12b7f5;
}

/* Email Tooltip 样式 */
.email-tooltip {
  position: absolute;
  left: 100%;
  top: 50%;
  transform: translateY(-50%);
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

.email-tooltip.success .tooltip-content {
  background-color: rgba(16, 185, 129, 0.9);
}

.tooltip-icon {
  font-size: 1.1em;
  color: #fff;
}

.tooltip-arrow {
  position: absolute;
  left: -6px;
  top: 50%;
  transform: translateY(-50%);
  width: 0;
  height: 0;
  border-top: 6px solid transparent;
  border-bottom: 6px solid transparent;
  border-right: 6px solid rgba(0, 0, 0, 0.85);
}

.email-tooltip.success .tooltip-arrow {
  border-right-color: rgba(16, 185, 129, 0.9);
}

/* Email Tooltip 动画 */
.email-tooltip-enter-active,
.email-tooltip-leave-active {
  transition: all 0.2s ease;
}

.email-tooltip-enter-from,
.email-tooltip-leave-to {
  opacity: 0;
  transform: translateY(-50%) translateX(-5px);
}

.email-tooltip-enter-to,
.email-tooltip-leave-from {
  opacity: 1;
  transform: translateY(-50%) translateX(0);
}

.info-icon {
  margin-right: 12px;
  color: #666;
  flex-shrink: 0;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.description-section,
.skills-section {
  text-align: left;
  margin-bottom: 20px;
}

.description-section h4,
.skills-section h4 {
  margin: 0 0 12px;
  color: #333;
  font-size: 1.1rem;
  font-weight: 600;
}

.description-section p {
  margin: 0;
  color: #555;
  line-height: 1.6;
  font-size: 0.9rem;
}

.skills-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.skill-tag {
  background-color: rgba(0, 0, 0, 0.1);
  color: #333;
  padding: 4px 12px;
  border-radius: 16px;
  font-size: 0.8rem;
  font-weight: 500;
}

.modal-footer {
  padding: 16px 24px 20px;
  border-top: 1px solid rgba(0, 0, 0, 0.1);
  text-align: center;
}

.copyright {
  margin: 0;
  color: #888;
  font-size: 0.8rem;
}

/* 过渡动画 */
.modal-enter-active {
  transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.modal-leave-active {
  transition: all 0.25s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container {
  transform: scale(0.8) translateY(-30px);
  opacity: 0;
}

.modal-leave-to .modal-container {
  transform: scale(0.95) translateY(10px);
  opacity: 0;
}

.modal-enter-to .modal-container,
.modal-leave-from .modal-container {
  transform: scale(1) translateY(0);
  opacity: 1;
}

/* 移动端适配 */
@media (max-width: 520px) {
  .modal-container {
    width: 95%;
    max-height: 85vh;
  }
  
  .modal-header,
  .modal-content,
  .modal-footer {
    padding-left: 16px;
    padding-right: 16px;
  }
  
  .avatar {
    width: 60px;
    height: 60px;
  }
  
  .skills-tags {
    justify-content: center;
  }
}
</style>
