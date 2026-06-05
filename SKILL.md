---
name: ant-design-skill
description: >
  基于产品 brief 构建可运行的前端原型。使用本地 `impeccable` 技能完成需求发现和规格文档（PRODUCT.md / DESIGN.md）生成，
  然后搭建 Vite + React + Ant Design + TypeScript 项目，并使用本地 `antd` 技能实现高设计质量的可运行原型。
  当用户要求"构建原型"、"搭建页面"、"创建应用"、"做个 demo"或提供产品创意并期望可运行演示时触发。
---

# 原型构建器

将产品创意转化为可运行的前端原型。本技能将需求发现和规格生成委托给本地 `impeccable` 技能，然后产出基于 Vite + React + antd + TypeScript 的可运行应用。

锁定技术栈：**npm + Vite + React 19 + Ant Design 6 + TypeScript 6 + Tailwindcss** — 详见 `assets/boilerplate/package.json`。

## 术语

- **工作目录**：当前技能被调用时所在的目录，即目标项目根目录。
- **SKILL 目录**：本技能自身所在的安装目录（`~/.claude/skills/ant-design-skill/`）。

> **路径约束（强制）**：本技能所有子技能（`impeccable`、`antd`）及资源文件（`assets/`、`references/`、`scripts/`）的查找，**必须严格限定在 SKILL 目录内部**，通过相对路径 `./` 引用。禁止到 SKILL 目录之外（如 `~/.claude/skills/` 根目录、全局 skills 目录或其他任意路径）查找同名技能或资源。
## 入口决策：从哪一步开始

根据工作目录的当前状态，按以下优先级判断起点：

| 状态 | 起点 |
|---|---|
| 工作目录为空，或缺少 `PRODUCT.md` / `DESIGN.md` | 从**步骤 1**开始，必须完成步骤 1–2 后才能进入步骤 3 |
| 已有 `PRODUCT.md` + `DESIGN.md`，但无前端项目 | **跳过步骤 1–2**，从**步骤 3**开始 |
| 前端项目已存在（有 `package.json` 和 `src/`） | **跳过步骤 3**。根据用户当前要求先更新 `PRODUCT.md` 和 `DESIGN.md`，然后从**步骤 4**开始 |

> **设计合规（步骤 6）和验证交付（步骤 7）在任何情况下都必须严格执行。**

## 工作流程（7 步）

### 步骤 1：生成 PRODUCT.md（impeccable init）
加载 SKILL 目录下（即当前技能安装目录内部）`./skills/impeccable/SKILL.md` 并运行 `init` 命令。禁止到 SKILL 目录之外查找 `impeccable`。

1. 扫描项目（如存在代码）以推断上下文
2. 使用简体中文向用户询问战略问题（register、用户、目的、品牌个性、反面参考、无障碍）
3. 在工作目录写入 `PRODUCT.md`

**约束：**
- `impeccable init` **仅生成 PRODUCT.md**，不会生成 DESIGN.md。
- 不要使用 `./references/design-questions.md` 进行需求发现 — 该文件仅作遗留参考。让 `impeccable` 主导访谈。
- 即使用户在提示中已提供足够上下文，`impeccable init` 仍会在写入前确认 register 和战略意图。

### 步骤 2：生成 DESIGN.md（impeccable document）

在 `PRODUCT.md` 存在后，加载 SKILL 目录下（即当前技能安装目录内部）`./skills/impeccable/reference/document.md` 并运行 `document` 命令。禁止到 SKILL 目录之外查找 `impeccable`。
`impeccable document` 是与 `init` **独立的步骤**，不会自动运行：

- **空项目 / 预实现项目**：以 seed 模式运行 — 通过简体中文询问 5 个高层问题（色彩策略、排版方向、动效能量、参考）并写入初始版 `DESIGN.md`
- **已有代码的项目**：以 scan 模式运行 — 从 CSS/Tailwind/配置中提取 token 并写入完整版 `DESIGN.md`

**约束：**
- 不要从 `./assets/templates/DESIGN.template.md` 复制 — `impeccable document` 负责 DESIGN.md 的生成。

### 步骤 3：搭建基础项目框架

#### 3.1 复制通用样板

将 SKILL 目录下的 `./assets/boilerplate/` 完整复制到工作目录。样板提供纯技术基础设施，不含任何业务页面：

- `package.json`
- `vite.config.ts`、`tsconfig.json`、`tsconfig.app.json`、`tsconfig.node.json`
- `index.html`、`eslint.config.js`
- `src/main.tsx` — 入口，含 `ConfigProvider` + `<AntdApp>` + zhCN 语言包
- `src/App.tsx` — 空占位组件（仅返回 `<div>App</div>`）
- `src/index.css` — 全局重置样式
- `src/vite-env.d.ts`

#### 3.2 安装依赖并验证构建

运行 `npm install`，并用 `npm run build` 验证（必须通过，0 错误）。

#### 3.3 初始化 Tailwind CSS

安装 Tailwind CSS Vite 插件：

```bash
npm install -D tailwindcss @tailwindcss/vite
```

修改以下文件完成集成：

- **`vite.config.ts`**：添加 `tailwindcss()` 插件：
  ```ts
  import tailwindcss from '@tailwindcss/vite'
  // ...
  plugins: [react(), tailwindcss()],
  ```

- **`src/index.css`**：在文件顶部添加 `@import "tailwindcss"`：
  ```css
  @import "tailwindcss";
  
  html, body, #root {
    height: 100%;
    margin: 0;
    padding: 0;
  }
  /* ...保留其余样式 */
  ```

再次运行 `npm run build` 验证 Tailwind 集成后构建仍通过（0 错误）。

> **优先级规则**：布局与间距优先使用 antd 组件和 token（`useToken()`）；Tailwind 仅用于 antd 无法快速覆盖的原子样式（如自定义 flex 布局、定位、尺寸微调）。若出现样式冲突，通过调整 `index.css` 中的 `@import` 和自定义规则顺序解决。

### 步骤 4：将规格文档转化为实现计划

**唯一输入来源**：工作目录的 `PRODUCT.md`（产品定义）和 `DESIGN.md`（设计规范）。不加入任何不在文件中的假设或推断。

**执行方式：**

1. 读取 `PRODUCT.md` —— 提取：
   - 页面/功能清单（Product Purpose、Out of Scope 划定的边界）
   - 用户角色与核心任务（Users 章节）
   - 关键交互与数据流（Open Questions 中已确认的部分）
   - 成功标准（验证实现是否满足产品目标）

2. 读取 `DESIGN.md` —— 提取：
   - 色彩 token 与主题配置
   - 排版层级（Display / Headline / Title / Body / Label 及对应字号/字重）
   - 间距标度（xs / sm / md / lg / xl / xxl 对应的 px 值）
   - 组件规格（卡片、按钮、表格、表单的圆角/阴影/内边距等）
   - 布局规则（网格系统、断点、最大宽度）

3. 综合两者，输出实现计划（可在脑中完成，也可写入临时笔记）：
   - 页面清单及区域划分（V1 聚焦 1–2 个页面）
   - 每个区域对应的 antd 组件
   - 各区域应使用的 DESIGN.md token（颜色、间距、排版层级）
   - 实现顺序（优先实现 PRODUCT.md 中定义的核心功能路径）

> **约束**：如果 `PRODUCT.md` 和 `DESIGN.md` 之间存在冲突（如产品要求 5 个页面但设计系统只定义了 3 种卡片样式），以 `PRODUCT.md` 的功能范围为准，用 `DESIGN.md` 的最近可用 token 做适配，并记录冲突点。

### 步骤 5：按规格文档增量实现（antd 技能）
在编写任何 antd 代码前，加载 SKILL 目录下（即当前技能安装目录内部）`./skills/antd/SKILL.md` 获取组件 API 详情。禁止到 SKILL 目录之外查找 `antd`。
按以下切片顺序增量构建，每个切片后验证：

1. **应用壳 + 主题** —— 按 `DESIGN.md` 的全局布局规则搭建页面框架；按 `PRODUCT.md` 的核心功能路径确定导航/入口结构。验证渲染和主题切换。
2. **核心页面骨架** —— 按 `PRODUCT.md` 的功能清单搭建各页面区域；按 `DESIGN.md` 的间距标度和布局规则排列。使用占位数据验证结构。
3. **关键交互组件** —— 实现 `PRODUCT.md` 中定义的核心交互（如表单提交、数据筛选、详情展开）；样式严格遵循 `DESIGN.md` 的组件规格和状态编码规则。
4. **数据展示与格式化** —— 填充 `PRODUCT.md` 中描述的数据结构；数字格式化、状态显示等遵循 `DESIGN.md` 的排版和编码规范。
5. **打磨** —— 响应式（按 `DESIGN.md` 断点）、暗色模式（`DESIGN.md` 主题配置）、边界情况（`PRODUCT.md` 中的空状态/异常场景）。

**每个切片后的验证：**
- `npm run build`（必须通过，0 错误）
- `npm run lint`（必须通过，0 警告）
- 在浏览器打开验证视觉效果
- 对照 `DESIGN.md` 逐项检查：颜色是否来自 token、间距是否匹配标度、排版层级是否正确

**编码约束：**
- 始终通过本地 `antd` 技能查询 API：编写代码前运行 `antd info <组件> --format json` 和 `antd demo <组件> basic --format json`
- 所有颜色使用 `useToken()` 获取 antd 主题 token，禁止 JSX 中硬编码颜色
- 使用 `ConfigProvider.useToken()` 在函数组件中访问 token
- 用 `theme.darkAlgorithm` / `theme.defaultAlgorithm` 实现主题切换
- 所有交互元素必须可被键盘访问
- 动画仅使用 `transform` 和 `opacity`，尊重 `prefers-reduced-motion`

### 步骤 6：设计系统合规（impeccable 验收）

为每个构建的组件对照工作目录的 `DESIGN.md` 检查：

- 颜色来自 token，而非硬编码
- 间距遵循 antd 标度（xs / sm / md / lg / xl / xxl）
- 排版使用层级（Display / Headline / Title / Body / Label）
- 状态使用颜色 + 图标 + 文本（三重编码）
- 暗色模式通过 `ConfigProvider` 工作

**验收流程（必须执行）：**
1. 加载 SKILL 目录下（即当前技能安装目录内部）`./skills/impeccable/SKILL.md`，阅读其命令文档。禁止到 SKILL 目录之外查找 `impeccable`。
2. 运行 `impeccable critique` 对原型页面进行设计打分
3. 对低分页面使用 `impeccable polish`（或 `layout` / `typeset` 等适当子命令）进行视觉优化

| 命令 | 用途 |
|---|---|
| `critique` | 对页面进行设计打分 |
| `audit` | 质量审查 |
| `polish` | 视觉优化 |
| `distill` | 提炼页面本质，去除复杂性 |
| `onboard` | 设计使用流程、空状态、激活引导 |
| `typeset` | 改善排版层次与字体 |
| `layout` | 修正间距、节奏与视觉层级 |
| `overdrive` | 增加个性与细节 |
| `extract` | 将可复用 token 和组件提取到 Design.md |

### 步骤 7：验证与交付

运行完整验证清单，全部通过后方可交付：

- [ ] `npm run build` 通过（0 错误）
- [ ] `npm run lint` 通过（0 警告）
- [ ] `npm run dev` 启动且无错误渲染
- [ ] 暗色模式切换工作正常
- [ ] 所有交互可点击并产生可见反馈
- [ ] 数字使用千分位分隔符和 2 位小数（如涉及数值展示）
- [ ] JSX 中无硬编码颜色（全部使用 `useToken()`）
- [ ] 控制台无警告或错误
- [ ] 清理项目根目录中临时生成的 `.png` 截图文件

交付内容：
- 在简短的 README 段落或提交信息中记录原型
- 向用户交付开发服务器 URL（`npm run dev` 后的本地地址）

## 资源清单

### `assets/boilerplate/`
纯技术基础设施的 Vite + React + antd + TypeScript 启动器，不含任何业务页面。在每个原型开始时复制到工作目录。

### `assets/templates/`（遗留参考 — 请勿直接复制）
- `PRODUCT.template.md` — 产品文档骨架参考
- `DESIGN.template.md` — 设计系统骨架参考（含 OKLCH 调色板、排版标度、组件规格）

### `references/`（按需加载）
- `design-principles.md` — 19 条适用于 B2B / 后台 / 仪表盘原型的通用设计原则
- `antd-component-guide.md` — 选择 antd 组件的决策树；补充 `antd` 技能
- `design-questions.md` — 遗留发现问题；仅保留作参考

### `skills/`（子技能）
- `./skills/impeccable/SKILL.md` — **步骤 1、2、6**：需求发现、规格生成、设计验收
- `./skills/antd/SKILL.md` — **步骤 5**：组件 API 详情、属性签名、演示代码

## 常见错误

| ❌ 不要做 | ✅ 正确做法 |
|---|---|
| 步骤 2 完成前开始编码 | 等待 `DESIGN.md` 就绪 |
| 绕过 `impeccable init` 手动提问 | 让 `impeccable` 的结构化访谈主导 |
| 直接复制示例 `PRODUCT.md` 或 `DESIGN.md` | 让 `impeccable` 从用户回答中生成 |
| 实现时脱离 `PRODUCT.md` 和 `DESIGN.md` 自由发挥 | 严格按两个文件的规格执行，功能来自 PRODUCT，样式来自 DESIGN |
| 在 JSX 中硬编码颜色 | 使用 `useToken()` 和 antd 主题 token |
| 未经询问引入新的 npm 依赖 | 锁定技术栈是故意的 |
| 跳过暗色模式检查 | 样板已配置好，验证它工作 |
| 构建 5 页应用 | 聚焦 1–2 个代表性页面 |
| 使用 hero 指标模板（大数字 + 小标签） | `DESIGN.md` §6 明确禁止 |
