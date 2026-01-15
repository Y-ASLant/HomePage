# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

Vue 3 + Vite 个人主页，展示个人信息、技能和联系方式。核心功能：Typed.js 打字机动画、一言 API 随机名言、毛玻璃风格 UI。

## 开发命令

```bash
npm install          # 安装依赖
npm run dev          # 开发服务器
npm run build        # 生产构建
npm run preview      # 预览构建结果
```

## 架构要点

### 状态管理
- **App.vue** 是状态中心：控制背景切换、移动端检测、AboutModal 显隐
- 组件间通过 props/emits 通信（如 AboutModal 的 visible/close）

### 外部依赖
- **iconify-icon**: 图标通过 CDN 引入，使用 Fluent、MDI 等图标集
- **@duskmoon/vue3-typed-js**: 打字机效果
- **一言 API**: `https://v1.hitokoto.cn/?max_length=20&c=a&c=b&c=c`

### 全局样式
- **main.css**: 定义 `floatUp` 动画、全局居中布局、禁用文本选择
- **字体**: MiSans VF（小米字体）+ JetBrains Mono（等宽代码字体）

### 响应式断点
- 768px: 移动端检测（禁用 tooltip）
- 560px: 布局调整（按钮仅显示图标、footer 间距调整）
- 520px: Modal 容器宽度适配

### 静态资源
- 背景图：`public/background/{0,1,2}.webp`（随机）
- 头像：`public/favicon.ico`
- 关于页：`public/about.png`

## 技术细节

- Vue API 混用：TypeJS.vue 用 Composition API + TS，其余用 Options API
- Email 复制有降级方案：`navigator.clipboard` → `document.execCommand('copy')`
- 毛玻璃效果统一：`backdrop-filter: blur(15px)` + `background-color: #00000043`
