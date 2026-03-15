<script>
export default {
  data() {
    return {
      // 使用 Iconify 的 Fluent 图标名称
      links: [
        { name: 'Blog', url: 'https://blog.aslant.top/', icon: 'fluent:book-24-regular' },
        { name: 'EMQX', url: 'https://mqtt.aslant.top/', icon: 'fluent:cloud-24-regular' },
        { name: '云盘', url: 'https://cloud.aslant.top/', icon: 'fluent:link-24-regular' },
        { name: 'GitHub', url: 'https://github.com/Y-ASLant', icon: 'mdi:github' }
      ]
    };
  }
};
</script>

<template>
  <div class="myButton-container">
    <a
      class="meBox-button"
      v-for="(link, i) in links"
      :key="i"
      :href="link.url"
      target="_blank"
      rel="noopener"
    >
      <iconify-icon :icon="link.icon" class="icon" height="1.35em" width="1.35em"></iconify-icon>
      <span class="label">{{ link.name }}</span>
    </a>
  </div>
</template>

<style scoped>
.myButton-container {
  display: flex;
  margin-top: 10px;
  justify-content: space-between;
}

.meBox-button {
  flex: 1;
  padding: 10px 0;
  border-radius: 10px;
  margin: 0 10px;
  background-color: #00000043;
  color: #fff;
  border: none;
  cursor: pointer;
  backdrop-filter: blur(15px);
  -webkit-backdrop-filter: blur(15px);
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  text-shadow: #00000043;
  animation: floatUp 1s ease-out;
  transition: background-color 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 16px;
}

.meBox-button .icon {
  width: 1.35em;
  height: 1.35em;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex: 0 0 auto; /* 不拉伸，保持固定尺寸 */
}

.meBox-button .icon svg {
  width: 100%;
  height: 100%;
  display: block;
}

.meBox-button .label {
  line-height: 1;
}

.meBox-button:hover {
  background-color: #00000038;
}

.meBox-button:first-child {
  margin-left: 0;
}

.meBox-button:last-child {
  margin-right: 0;
}

/* 移动端仅显示图标，隐藏文本；图标稍微变大一点，点击区域不变 */
@media (max-width: 520px) {
  .meBox-button {
    gap: 0;
  }
  .meBox-button .label {
    display: none;
  }
  .meBox-button .icon {
    width: 1.6em;
    height: 1.6em;
  }
}
</style>
