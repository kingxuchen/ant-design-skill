---
name: [产品名]
description: [一句话产品定位, 例: XX 场景下的 YY 工具]
colors:
  primary: "#1677ff"
  primary-deep: "#0958d9"
  primary-light: "#e6f4ff"
  success: "#52c41a"
  warning: "#faad14"
  error: "#f5222d"
  info: "#1677ff"
  text: "#000000"
  text-secondary: "#595959"
  text-tertiary: "#8c8c8c"
  background: "#ffffff"
  surface: "#f5f5f5"
  surface-elevated: "#fafafa"
  border: "#d9d9d9"
  border-light: "#f0f0f0"
typography:
  display: { fontSize: "24px", fontWeight: 600, lineHeight: 1.4, letterSpacing: "-0.02em" }
  headline: { fontSize: "20px", fontWeight: 600, lineHeight: 1.4, letterSpacing: "-0.01em" }
  title: { fontSize: "16px", fontWeight: 500, lineHeight: 1.5 }
  body: { fontSize: "14px", fontWeight: 400, lineHeight: 1.5715 }
  label: { fontSize: "12px", fontWeight: 500, lineHeight: 1.5, letterSpacing: "0.01em" }
rounded: { sm: "4px", md: "6px", lg: "8px", xl: "12px", full: "9999px" }
spacing: { xs: "4px", sm: "8px", md: "16px", lg: "24px", xl: "32px", xxl: "48px" }
components:
  button-primary: { backgroundColor: "{colors.primary}", textColor: "{colors.background}", rounded: "{rounded.md}", height: "32px" }
  button-default: { backgroundColor: "{colors.background}", textColor: "{colors.text}", border: "1px solid {colors.border}", rounded: "{rounded.md}" }
  card: { backgroundColor: "{colors.background}", rounded: "{rounded.lg}", padding: "24px" }
  input: { backgroundColor: "{colors.background}", border: "1px solid {colors.border}", rounded: "{rounded.md}", height: "32px" }
  alert-warning: { backgroundColor: "#fffbe6", textColor: "#d48806", rounded: "{rounded.md}" }
---

# Design System: [产品名]

## 1. Overview

**Creative North Star**: [一句话形容整体气质, 例: "The Clean Workshop" — 工具消失进背景, 让数据说话]

参考: [Linear](https://linear.app), [Notion](https://notion.so) — 现代友好精确; 拒绝营销页夸张、传统 ERP 笨重。

## 2. Colors

### 主色板 (OKLCH 色彩空间, 确保明暗主题下感知亮度一致)

```
Primary:    #1677ff (Light) / #0958d9 (Deep) / #e6f4ff (Tint)
Success:    #52c41a / #3f8600 / #f6ffed
Warning:    #faad14 / #d48806 / #fffbe6
Error:      #f5222d / #cf1322 / #fff1f0
```

### 中性色

```
Ink:        #1a1a1a (Light) / #f0f0f0 (Dark)
Secondary:  #595959 / #a0aec0
Tertiary:   #8c8c8c / #6b7280
Muted:      #bfbfbf / #4b5563
Background: #ffffff / #111827
Surface:    #f5f5f5 / #0a0e1a
Border:     #d9d9d9 / rgba(255,255,255,0.06)
```

### 命名规则

- **The One Voice Rule**: 主色 blue 仅用于 primary 操作、当前选中、链接。装饰用途严格禁止。
- **The Semantic State Rule**: 成功/警告/错误三色仅用于对应语义状态。
- **Contrast**: 文本对比度 ≥ 4.5:1 (正文) / 3:1 (大文本)。

## 3. Typography

**字体栈**: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif`

### 字号阶梯 (1.125-1.2 比例)

| 层级 | Size | Weight | 行高 | Letter Spacing | 用途 |
|---|---|---|---|---|---|
| Display | 24px | 600 | 1.4 | -0.02em | 页面标题、大数字 |
| Headline | 20px | 600 | 1.4 | -0.01em | 区块标题、卡片标题 |
| Title | 16px | 500 | 1.5 | normal | 子标题、列头 |
| Body | 14px | 400 | 1.5715 | normal | 正文、表格内容 |
| Label | 12px | 500 | 1.5 | 0.01em | 表单标签、徽章 |

**规则**: 单一字体栈, 靠字重 (400/500/600) 建层级, 不换字体。

## 4. Elevation

Subtle Layered: 卡片默认微阴影, hover 抬升。不用 backdrop-filter / 毛玻璃。

```
Card Rest:   0 1px 2px 0 rgba(0,0,0,0.03), 0 1px 6px -1px rgba(0,0,0,0.02), 0 2px 4px 0 rgba(0,0,0,0.02)
Card Hover:  0 4px 12px 0 rgba(0,0,0,0.08), 0 2px 8px -2px rgba(0,0,0,0.04)
Header:      0 1px 4px rgba(0,0,0,0.1)
```

## 5. Layout

- **Grid**: CSS Grid 2D 布局, Flexbox 1D 布局
- **Max width**: 1400px 居中
- **响应式断点**:
  - Mobile: < 768px
  - Tablet: 768px – 1200px
  - Desktop: 1200px+
- **Z-Index 语义化**:
  ```
  dropdown:       1000
  sticky:         1020
  modal-backdrop: 1040
  modal:          1050
  toast:          1060
  tooltip:        1070
  ```

## 6. Components

### Buttons
- Primary: 蓝色背景 #1677ff + 白字, hover 加深 #0958d9
- Default: 白底 + 灰边 #d9d9d9 + 深字, hover 边变 #4096ff
- 圆角 6px, 高 32px
- 状态完整: default / hover / focus / active / disabled / loading / error

### Cards
- 8px 圆角, 白底
- 24px 标准内边距
- 默认阴影, hover 抬升
- **No Nested Cards**: 不在卡片内嵌次级卡片

### Inputs
- 32px 高, 6px 圆角
- Focus: 边框 #1677ff + 2px 蓝色 outline
- Error: 边框 #f5222d

### Alert
- Warning: 黄底 #fffbe6 + 深黄文字 #d48806
- **不用 border-left 彩色条**, 用完整边框或背景色块

### Table
- Header: Title 层级, 灰底 #fafafa
- Row hover: 背景 #fafafa
- Summary Row: 粗体 + 上边框
- Skeleton Loading: 灰色脉冲块

### Statistic
- Value: Display 24px / 600
- Label: Label 12px / 500 / #595959
- 千分位分隔、保留两位小数
- **No Hero-Metric**: 避免大数字+小标签的 SaaS cliché

## 7. Motion

- 过渡 150-250ms, ease-out-quart
- 优先 `transform` / `opacity`, 避免 `width` / `height` / `padding` 动画
- 所有动画支持 `prefers-reduced-motion`

## 8. Dark Mode

用 antd `ConfigProvider.algorithm = darkAlgorithm`, 不手写 CSS 变量。

```tsx
<ConfigProvider theme={{ algorithm: isDark ? theme.darkAlgorithm : theme.defaultAlgorithm }}>
```

- 自定义卡片背景: `token.colorBgContainer`
- 自定义边框: `token.colorBorderSecondary`
- 主题切换: 顶部导航栏右侧, 🌙/☀️ 图标, localStorage 持久化

## 9. Do's and Don'ts

**Do**:
- 系统字体栈
- 蓝色仅用于 primary 操作
- 千分位分隔大数字
- 两位小数金额
- 图标+颜色+文字三重编码状态
- 24px 卡片 padding
- 所有交互状态视觉
- 对比度验证
- transform/opacity 动画
- prefers-reduced-motion 支持
- CSS Grid 2D + Flexbox 1D
- 语义化 z-index

**Don't**:
- border-left side-stripe
- 渐变文字
- 毛玻璃
- 大数字+小标签 hero-metric
- 完全相同的卡片网格 × N
- 区块上方大写 eyebrow 标签
- 营销术语 (streamline/empower/...)
- 文本溢出容器
- 装饰性动效
- 未选中状态用高饱和色
- width/height/padding 动画
- arbitrary z-index
- 正文 all-caps
- em dashes (—)
- 营销套话

## 10. Code Quality

### CSS
- 用 CSS 变量或 antd token, 不用硬编码颜色
- 暗色走 `ConfigProvider`
- 动画仅用 transform / opacity
- 断点: 768px, 1200px
- 最小触控目标: 44×44px

### TypeScript / React
- 组件 props 用 interface
- token 通过 `useToken()` 钩子
- 主题状态用 React Context
- 组件支持 loading 状态

### Performance
- 不在滚动时用模糊/阴影动画
- will-change 谨慎使用
- 图片用 WebP
- 代码分割: 路由级 + 大型组件
