# 💬 SpeechMeme.org

> 轻量级语音气泡梗图生成器 - 在任意图片上添加语音气泡和文字对白，快速生成可分享的 Meme PNG 图片

## ✨ 特性

- 🎨 **气泡编辑** - 支持 Rounded/Rectangle/Bruh 三种气泡样式
- 📝 **文字编辑** - 在气泡内添加自定义文字，支持多语言
- 🎯 **一键复制** - 直接复制图片到剪贴板，无需下载
- 💾 **本地导出** - 导出高质量 PNG 图片（透明背景）
- 🖼️ **作品展示** - Gallery 展示无文字的用户作品
- 🚀 **纯前端** - 无需服务器，图片处理完全在本地进行
- 📱 **响应式** - 支持桌面和移动设备

## 🛠️ 技术栈

- **框架**: Next.js + React
- **样式**: Tailwind CSS
- **渲染**: Canvas 2D API
- **状态管理**: useState + useRef
- **存储**: localStorage（保存设置）
- **部署**: Vercel（静态网站）

## 📦 项目结构

```
speech-meme/
├── src/
│   ├── components/       # React 组件
│   │   ├── BubbleEditor.tsx    # 气泡编辑器
│   │   ├── Canvas.tsx          # 画布组件
│   │   ├── TextEditor.tsx      # 文字编辑器
│   │   └── ExportPanel.tsx     # 导出面板
│   ├── hooks/           # 自定义 Hooks
│   │   ├── useCanvas.ts        # Canvas 操作
│   │   └── useClipboard.ts     # 剪贴板功能
│   ├── utils/           # 工具函数
│   │   ├── image.ts            # 图片处理
│   │   ├── bubble.ts           # 气泡绘制
│   │   └── export.ts           # 导出功能
│   └── styles/          # 样式文件
├── public/              # 静态资源
├── pages/               # Next.js 页面
├── .gitignore
├── README.md
├── package.json
├── next.config.ts
├── tailwind.config.ts
└── tsconfig.json
```

## 🚀 快速开始

### 环境要求

- Node.js 18.0 或更高版本
- npm 或 yarn

### 安装依赖

```bash
npm install
```

### 开发环境

```bash
npm run dev
```

访问 [http://localhost:3000](http://localhost:3000) 查看应用。

### 构建部署

```bash
# 构建生产版本
npm run build

# 启动生产服务器
npm start
```

## 📖 使用指南

### 基本使用流程

1. **上传图片** - 拖拽或点击上传图片（支持 JPG/PNG/WEBP，最大 5MB）
2. **添加气泡** - 选择气泡样式，点击画布添加气泡
3. **编辑文字** - 点击气泡输入文字，支持自定义字体和颜色
4. **调整位置** - 拖拽气泡到合适位置
5. **导出分享** - 生成图片后可直接复制到剪贴板或下载保存

### 气泡样式

- **Rounded** - 圆角气泡，适合日常对话
- **Rectangle** - 方形气泡，适合正式场景
- **Bruh** - 特殊造型气泡，用于表达强烈情绪

### 高级功能

- 透明背景模式
- 自定义字体大小和颜色
- 尾巴方向调整（左/中/右）
- 多气泡支持（最多2个）

## 🔧 开发说明

### 核心组件

- **BubbleEditor** - 气泡样式编辑器，管理气泡类型、尾巴方向、透明度等设置
- **Canvas** - 主画布组件，处理图片渲染、气泡绘制、文字显示
- **TextEditor** - 文字编辑器，提供字体、大小、颜色等文字样式设置
- **ExportPanel** - 导出面板，提供复制和下载功能

### 关键功能实现

#### 气泡绘制

使用 Canvas 2D API 绘制气泡，支持三种样式和尾巴方向调整：

```typescript
// 绘制圆角气泡
function drawRoundedBubble(ctx, x, y, width, height, radius) {
  ctx.beginPath();
  ctx.moveTo(x + radius, y);
  ctx.arcTo(x + width, y, x + width, y + height, radius);
  // ... 绘制其他边和角
}
```

#### 文字渲染

文字渲染采用 Canvas 文字 API，支持自动换行和多语言字符：

```typescript
// 渲染文字
function renderText(ctx, text, x, y, maxWidth, fontSize, font) {
  ctx.font = `${fontSize}px ${font}`;
  ctx.fillStyle = textColor;

  // 自动换行处理
  const lines = wrapText(ctx, text, maxWidth);
  lines.forEach((line, index) => {
    ctx.fillText(line, x, y + (index + 1) * fontSize);
  });
}
```

#### 剪贴板复制

使用现代浏览器的 Clipboard API 实现一键复制：

```typescript
// 复制图片到剪贴板
async function copyImageToClipboard(canvas) {
  try {
    const blob = await new Promise(resolve => canvas.toBlob(resolve));
    await navigator.clipboard.write([
      new ClipboardItem({ 'image/png': blob })
    ]);
    showToast('图片已复制到剪贴板！');
  } catch (error) {
    showToast('复制失败，请使用下载功能');
  }
}
```

## 🎨 设计理念

### 视觉风格

- **暗色主题** - 采用 Discord 风格的暗紫灰背景 (#1E1E2F)
- **亮色强调** - 使用亮紫蓝 (#6C63FF) 作为主要交互色
- **圆润设计** - 统一使用 16px 圆角，营造柔和友好感
- **发光效果** - 按钮和交互元素添加柔光阴影

### 用户体验

- **零学习成本** - 界面直观，无需教程即可上手
- **即时反馈** - 所有操作都有视觉和文字反馈
- **容错设计** - 支持撤销和重置操作
- **隐私保护** - 所有处理都在本地进行，不上传用户数据

## 📊 功能路线图

### MVP v1.0 (当前版本)

- [x] 基础气泡编辑功能
- [x] 文字编辑功能
- [x] 图片导出功能
- [x] 剪贴板复制功能
- [x] Gallery 展示

### v1.1 (计划中)

- [ ] 更多气泡样式
- [ ] 图片滤镜效果
- [ ] GIF 格式支持
- [ ] 项目保存/加载

### v2.0 (未来版本)

- [ ] 用户账户系统
- [ ] 云端作品存储
- [ ] 协作编辑
- [ ] AI 辅助创作

## 🤝 贡献指南

我们欢迎所有形式的贡献！

### 开发流程

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 创建 Pull Request

## 🙏 致谢

- 感谢 [speechmeme.com](https://speechmeme.com) 提供的产品灵感
- 感谢所有 Discord 社区的早期用户和反馈者
- 感谢 Next.js 和 Tailwind CSS 团队提供优秀的开发工具

## 📞 联系我们

- 官网: [SpeechMeme.org](https://speechmeme.org)
- 问题反馈: [GitHub Issues](https://github.com/your-repo/speech-meme/issues)
- 邮箱: hello@speechmeme.org

---

<div align="center">
  <p>用 ❤️ 和创意制作，为全世界的梗图创作者服务</p>
  <p>Made with ❤️ by creators, for creators</p>
</div>
