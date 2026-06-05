# Prototype Builder

> 基于产品 brief 构建单页后台/仪表盘原型的 Claude Skill。

## 简介

**Prototype Builder** 将产品创意转化为可运行的单页后台/仪表盘原型。它通过结构化的需求发现、设计规格生成和增量式实现，确保每个原型都具备高设计质量和工程规范。

## 技术栈

| 层级 | 技术 | 版本 |
|---|---|---|
| 构建工具 | Vite | ^8.0 |
| 框架 | React | ^19.2 |
| UI 库 | Ant Design | ^6.4 |
| 语言 | TypeScript | ~6.0 |
| 包管理 | npm | — |

> **V1 约束**：不引入额外的 UI 库、状态管理器或路由方案。

## 目录结构

```
prototype-builder/
├── SKILL.md                      # Skill 主定义（工作流、命令、资源引用）
├── README.md                     # 本文件
├── skills/
│   ├── impeccable/               # 需求发现 & 设计规格生成
│   ├── antd/                     # Ant Design 组件 API 查询与实现指南
│   └── frontend-design/          # 前端设计辅助
├── assets/
│   ├── boilerplate/              # Vite + React + antd + TS 启动模板
│   │   ├── package.json
│   │   ├── vite.config.ts
│   │   ├── tsconfig*.json
│   │   ├── index.html
│   │   ├── eslint.config.js
│   │   └── src/
│   │       ├── main.tsx          # 入口（含 ConfigProvider + AntdApp + zhCN）
│   │       ├── App.tsx           # 占位组件
│   │       ├── index.css         # 全局重置样式
│   │       └── vite-env.d.ts
│   └── templates/                # 遗留参考模板（PRODUCT / DESIGN）
├── references/
│   ├── design-principles.md      # 19 条 B2B / 后台 / 仪表盘通用设计原则
│   ├── antd-component-guide.md   # antd 组件选择决策树
│   └── design-questions.md       # 遗留发现问题（参考用）
└── scripts/                      # 工具脚本
```

## 工作流程

原型构建遵循 **7 步流水线**，必须按顺序执行：

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  步骤 1     │ → │  步骤 2     │ → │  步骤 3     │
│  Impeccable │    │  Impeccable │    │  搭建项目   │
│  Init       │    │  Document   │    │  (Boilerplate)
│  PRODUCT.md │    │  DESIGN.md  │    │             │
└─────────────┘    └─────────────┘    └─────────────┘
                                              ↓
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  步骤 7     │ ← │  步骤 6     │ ← │  步骤 4/5   │
│  验证交付   │ ← │  设计合规   │ ← │  增量构建   │
│             │    │  (Polish)   │    │  (antd skill)
└─────────────┘    └─────────────┘    └─────────────┘
```

### 步骤详解

| 步骤 | 动作 | 输出 | 关键约束 |
|---|---|---|---|
| **1** | 运行 `impeccable init` | `PRODUCT.md` | 用户必须回答完战略问题后才能继续 |
| **2** | 运行 `impeccable document` | `DESIGN.md` | 独立步骤，不会自动运行 |
| **3** | 复制 `assets/boilerplate/` → 工作目录，执行 `npm install` | 可构建项目 | `npm run build` 必须通过 |
| **4** | 基于 `PRODUCT.md` 规划页面结构 | 页面清单 | 聚焦 1-2 个代表性页面 |
| **5** | 使用 `antd` skill 增量实现 | 可运行原型 | 每切片后 `build` + `lint` |
| **6** | `impeccable critique` 打分 + `polish` 优化 | 高设计质量 UI | 对照 `DESIGN.md` 检查 token、间距、排版 |
| **7** | 完整验证清单 + 交付 | 开发服务器 URL | 暗色模式、交互、控制台零错误 |

### 入口条件判断

- **空项目 / 无 PRODUCT.md & DESIGN.md** → 从步骤 1 开始
- **有规格文档但无前端项目** → 跳过步骤 1-2，从步骤 3 开始
- **项目已存在** → 先更新规格文档，再进入步骤 4

## 核心资源

### 设计原则（19 条）

所有原型必须遵循 `references/design-principles.md` 中的通用原则，包括：

- **信息密度与呼吸感平衡** — 卡片 24px padding，24px 间距
- **状态三重编码** — 颜色 + 图标 + 文字，绝不单独使用颜色
- **操作前置** — 高频操作放在最显眼位置
- **数字精确感** — 千分位分隔符、2 位小数、`font-variant-numeric: tabular-nums`
- **零摩擦切换** — 视图切换后数据联动刷新
- **无障碍基线** — WCAG 2.1 AA、`prefers-reduced-motion`、键盘可访问

### 子技能引用

| 子技能 | 路径 | 用途 |
|---|---|---|
| **impeccable** | `./skills/impeccable/SKILL.md` | 步骤 1-2：需求发现、规格生成；步骤 6：设计验收 |
| **antd** | `./skills/antd/SKILL.md` | 步骤 5：组件 API、属性签名、Demo 查询 |

#### Impeccable 可用命令

```
impeccable critique    # 对页面进行设计打分
impeccable audit       # 质量审查
impeccable polish      # 视觉优化
impeccable distill     # 提炼本质，去除复杂性
impeccable onboard     # 设计使用流程、空状态、激活引导
impeccable typeset     # 改善排版层次与字体
impeccable layout      # 修正间距、节奏与视觉层级
impeccable overdrive   # 增加个性与细节
impeccable extract     # 提取可复用 token 和组件到 Design.md
```

## 常见错误与规避

| ❌ 不要 | ✅ 替代方案 |
|---|---|
| 步骤 2 完成前开始编码 | 等待 `DESIGN.md` 就绪 |
| 绕过 `impeccable init` 手动提问 | 让该技能的结构化访谈主导 |
| 直接复制示例 `PRODUCT.md` / `DESIGN.md` | 从用户回答中生成 |
| JSX 中硬编码颜色 | 使用 `useToken()` 和 antd 主题 token |
| 未经询问引入新 npm 依赖 | 锁定技术栈是故意的 |
| 跳过暗色模式检查 | 样板已配置，验证它工作 |
| 构建 5 页应用 | 聚焦 1-2 个代表性页面 |
| 使用 hero 指标模板（大数字 + 小标签） | `DESIGN.md` §6 明确禁止 |

## 触发条件

当用户提出以下请求时激活本 Skill：

- "为 X 构建原型"
- "搭建仪表盘"
- "创建管理面板"
- 提供产品创意并期望可运行演示

## 验证清单（步骤 7）

交付前必须全部通过：

- [ ] `npm run build` 通过（0 错误）
- [ ] `npm run lint` 通过（0 警告）
- [ ] `npm run dev` 启动且无错误渲染
- [ ] 暗色模式切换工作
- [ ] 所有交互可点击并产生可见反馈
- [ ] 数字使用千分位分隔符和 2 位小数
- [ ] JSX 中无硬编码颜色（使用 `useToken()`）
- [ ] 控制台无警告或错误
- [ ] 清理生成的 `.png` 截图资源

---

*Maintained as part of the Oh My Pi skill ecosystem.*
