# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

Learn more about IDE Support for Vue in the [Vue Docs Scaling up Guide](https://vuejs.org/guide/scaling-up/tooling.html#ide-support).

# 佳豪学校官网

一个类似 Apple 官网的一镜到底效果网站，展示佳豪学校的相关信息。

## 功能特性

- 🎨 一镜到底的滚动效果
- 📱 响应式设计，适配各种设备
- 🗺️ 集成高德地图显示学校位置
- 🖼️ 图片灯箱查看功能
- ✨ 丰富的动画和交互效果

## 技术栈

- Vue 3
- Vite
- 高德地图 API

## 安装和运行

```bash
# 安装依赖
npm install

# 启动开发服务器
npm run dev

# 构建生产版本
npm run build
```

## 高德地图配置

要使用地图功能，你需要：

1. 注册高德开发者账号：https://lbs.amap.com/
2. 创建应用并获取 API 密钥
3. 在 `src/App.vue` 文件中替换 `YOUR_AMAP_KEY` 为你的实际 API 密钥

```javascript
// 在 loadAMap 函数中替换
script.src = "https://webapi.amap.com/maps?v=1.4.15&key=YOUR_AMAP_KEY";
```

## 项目结构

```
src/
├── App.vue              # 主应用组件
├── components/          # 组件目录
│   ├── ParallaxSection.vue
│   └── ScrollIndicator.vue
├── assets/             # 静态资源
│   ├── members/        # 成员照片
│   └── volunteer-pictures/  # 活动照片
└── style.css           # 全局样式
```

## 功能说明

### 首页

- 欢迎信息和背景图片
- 平滑滚动提示

### 关于我们

- 团队成员展示（2 列网格布局）
- 学校介绍图片

### 图片展示

- 16 张活动照片展示
- 4x4 网格布局
- 点击查看大图功能

### 联系我们

- 高德地图显示学校位置
- 联系信息卡片
- 响应式布局

## 注意事项

- 确保所有图片资源都存在于 `src/assets` 目录中
- 高德地图需要有效的 API 密钥才能正常工作
- 建议在生产环境中使用 HTTPS 协议
