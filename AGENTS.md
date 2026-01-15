# AGENTS.md

此文件包含代理在此代码库中工作时所需的重要信息和指南。

## 项目概述

Vue 3 + Vite 个人主页网站，使用 ES Modules，支持响应式设计。

## 构建与开发命令

```bash
# 安装依赖
npm install

# 开发服务器
npm run dev

# 生产构建
npm run build

# 预览生产构建
npm run preview
```

**注意**：此项目未配置测试套件。

## 代码风格指南

### Vue 组件

**混合使用 Composition API 和 Options API**：
- 优先使用 `<script setup>` 语法糖（如 Hitokoto.vue, Copyrights.vue）
- 复杂组件可使用 Options API `export default`（如 App.vue, AboutModal.vue）
- 仅在需要类型定义时使用 TypeScript（如 TypeJS.vue）

**组件命名**：
- 文件名：PascalCase（如 `MyButton.vue`, `AboutModal.vue`）
- 组件引用：PascalCase（如 `<MyButton />`）

### 命名约定

- **变量/函数**：camelCase（`showAboutModal`, `handleShowAbout`）
- **CSS 类名**：kebab-case（`meBox-container`, `modal-overlay`）
- **常量**：UPPER_SNAKE_CASE（仅在必要时使用）
- **数据属性**：描述性名称（`backgroundImages`, `currentBackground`）

### 导入规范

```javascript
// Vue 核心 API
import { createApp, ref } from 'vue'

// 本地组件使用 @ 别名
import MyButton from '@/components/MyButton.vue'

// 第三方库
import { Typed } from "@duskmoon/vue3-typed-js"
```

### 类型系统

**TypeScript 使用**：
- 大部分代码使用 JavaScript
- 仅在需要类型安全时使用 `<script setup lang="ts">`
- 如添加类型定义，导入类型：`import type { TypedOptions } from "..."`
- **禁止**使用 `@ts-ignore`, `@ts-expect-error` 抑制类型错误

### 样式规范

**CSS 架构**：
- 组件级样式使用 `<style scoped>`
- 全局样式在 `src/assets/main.css`
- 使用响应式断点：移动端 `max-width: 768px`, 桌面端默认

**常用样式模式**：
```css
/* 毛玻璃效果 */
backdrop-filter: blur(15px);
-webkit-backdrop-filter: blur(15px);

/* 阴影 */
box-shadow: 0 0 10px rgba(0, 0, 0, 0.25);

/* 自定义动画 floatUp（在 main.css 定义） */
animation: floatUp 1s ease-out;

/* 过渡效果 */
transition: all 0.3s ease;
```

**移动端适配**：
- 关键断点：520px, 560px, 768px
- 移动端隐藏 tooltip，仅显示图标
- 隐藏滚动条：`scrollbar-width: none`

### 组件模式

**Props 定义**（Options API）：
```javascript
props: {
  visible: {
    type: Boolean,
    default: false
  }
}
```

**Emits**：
```javascript
emits: ['close'],
methods: {
  closeModal() {
    this.$emit('close');
  }
}
```

**生命周期钩子**：
```javascript
mounted() {
  this.init();
},
beforeUnmount() {
  this.cleanup();
}
```

### 外部资源

**图标**：
- 使用 Iconify CDN：`https://code.iconify.design/iconify-icon/2.1.0/iconify-icon.min.js`
- 图标集：Fluent（`fluent:icon-name`）、MDI（`mdi:icon-name`）、Logos

**字体**：
- 中文：MiSans VF（小米字体）
- 代码：JetBrains Mono

### 错误处理

```javascript
async function fetchData() {
  try {
    const response = await fetch('url');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('请求失败:', error);
    // 降级方案或默认值
    return defaultValue;
  }
}
```

### 事件处理

**命名规范**：
- 事件处理器：`handle + 事件名`（`handleClick`, `handleMouseEnter`）
- 方法：动词开头（`copyEmail`, `closeModal`）

**移动端适配**：
```javascript
checkIfMobile() {
  this.isMobile = window.innerWidth <= 768 || 'ontouchstart' in window;
}
```

## 路径别名

项目配置了以下路径别名（在 `vite.config.js` 中定义）：
- `@` → `src/`

导入本地文件时优先使用别名：
```javascript
import MyComponent from '@/components/MyComponent.vue'
```

## 响应式断点

- **移动端**：≤ 768px
- **小屏幕**：≤ 560px
- **极小屏幕**：≤ 520px（按钮仅显示图标）

## 注意事项

1. **无测试套件**：项目未配置测试框架
2. **无 Linter**：项目未配置 ESLint/Prettier，手动保持代码一致性
3. **类型安全**：仅在必要时使用 TypeScript
4. **浏览器兼容性**：需支持 `-webkit-` 前缀（Safari）
5. **图片资源**：静态资源放在 `public/` 目录，使用 `/` 引用
6. **动画性能**：优先使用 transform 和 opacity（触发 GPU 加速）

## 代码组织

```
src/
├── assets/         # 全局资源（CSS、图片）
├── components/     # Vue 组件
├── main.js         # 应用入口
└── App.vue         # 根组件
```

组件职责单一，文件大小控制在合理范围（参考现有组件）。
