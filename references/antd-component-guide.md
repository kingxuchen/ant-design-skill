# Ant Design Component Guide

When building a prototype, choose antd components by purpose. This guide is the decision tree; the `antd` skill is the live API reference.

## Loading a New Instance

If the `antd` skill is available in this Codex session, **load it first** for current API details, props, and examples. Use this guide for selection decisions only.

## Component Selection

### Layout & Structure

| Need | Component | Notes |
|---|---|---|
| Page shell | `Layout` + `Header` + `Content` | No `Sider` for single-page dashboards |
| Top bar | `Layout.Header` (sticky, shadow) | Logo + title + actions |
| Card / container | `Card` | 8px radius, white bg, shadow |
| Grid system | `Row` + `Col` (xs/sm/md/lg/xl) | For responsive layout |
| Divider | `Divider` | Vertical in inline groups, horizontal between sections |
| Empty state | `Empty` | Always with `image={Empty.PRESENTED_IMAGE_SIMPLE}` |
| Skeleton | `Skeleton` | For loading states, not spinners |

### Data Display

| Need | Component | Notes |
|---|---|---|
| Big number | `Statistic` | Prefix (¥) + suffix (万) + valueStyle for color |
| Table | `Table` | columns, sorter, summary row, pagination prop |
| Tag / Badge | `Tag` (color states) / `Badge` (count) | Never border-left stripe |
| Progress | `Progress` (linear) or hand-rolled SVG `CircularProgress` | Transform scaleX, not width |
| Tooltip | `Tooltip` | Title prop, hover trigger |
| Avatar | `Avatar` | With icon when no image |

### Input & Selection

| Need | Component | Notes |
|---|---|---|
| Single select | `Select` with `showSearch` for > 7 options | `popupMatchSelectWidth={false}` for long labels |
| Cascading select | `Cascader` (tenant → environment) | Two-level: parent options + children |
| Date range | `DatePicker.RangePicker` | Dayjs auto-bundled by antd 6 |
| Switch | `Switch` | For boolean toggles |
| Form fields | `Form` + `Input`/`InputNumber` | When persistence is needed |

### Buttons & Actions

| Need | Component | Notes |
|---|---|---|
| Primary action | `Button type="primary"` | One per visible area |
| Danger action | `Button type="primary" danger` | For destructive operations |
| Default action | `Button` (no type) | Secondary actions |
| Icon button | `Button icon={...}` | 36x36 for icon-only |
| Button group | `Space size="middle"` | Between buttons |

### Feedback

| Need | Component | Notes |
|---|---|---|
| Status message | `message.success()` / `.error()` / `.warning()` | Static API, requires `<App>` wrap |
| Confirmation | `Modal.confirm()` | For destructive actions |
| Information popup | `Modal.info()` | For stubs and notifications |
| Page-level result | `Result` (success/error/404) | For empty/error states |
| Inline alert | `Alert` (warning/info/error/success) | With `showIcon` and `action` slot |
| Loading indicator | `Spin` or `Skeleton` | Prefer Skeleton for content, Spin for actions |

### Navigation

| Need | Component | Notes |
|---|---|---|
| Tabs (content switch) | `Tabs` | For view-mode within a page |
| Segmented (2-3 options) | `Segmented` | Better than radio for "总览 / 明细" |
| Breadcrumb | `Breadcrumb` | For multi-level navigation |
| Steps | `Steps` | For wizard/multi-step flows |
| Pagination | `Pagination` (in Table) | When > 10 rows |

### Data Entry (forms)

| Need | Component | Notes |
|---|---|---|
| Single line text | `Input` | With `prefix` / `suffix` for icons |
| Multi-line text | `Input.TextArea` | Auto-size when needed |
| Number | `InputNumber` | With min/max/step |
| Auto-complete | `AutoComplete` | For known option lists |
| Upload | `Upload` | For file inputs |

## Component Pairings

| Pattern | Components |
|---|---|
| Big-stat card | `Card` + `Statistic` + `Badge` (state) |
| Filterable list | `Input.Search` + `Select` + `Table` |
| Confirmation dialog | `Modal` + `Form` + `Button group` |
| Page header | `Layout.Header` + `Title` + `Space` (actions) + `Tooltip` (icons) |
| Stat row | 4× `MiniCard` in CSS Grid `repeat(4, 1fr)` |
| Status badge row | `Tag` with `color` prop in a column |

## Anti-Patterns to Avoid

- **Don't** compose `Card` inside `Card` (use divider or spacing)
- **Don't** use `Table` for layout (use Grid/Flexbox)
- **Don't** use `Statistic` without label
- **Don't** use multiple `Button type="primary"` next to each other
- **Don't** add icon-only buttons without `Tooltip`
- **Don't** use `Modal` for confirmations where `Popconfirm` is enough
- **Don't** use `Tag` color="red" without text/icon (color-only state)

## When to Deviate from antd

- Need a circular progress that animates: roll a small SVG component (use `transform: scaleX` for the bar)
- Need a complex custom chart: roll your own or bring in a chart lib (out of scope for V1)
- Need a complex editor: bring in a specialized lib (out of scope)
- Need drag-and-drop: use `@dnd-kit` (added separately, not in antd)

## Theme Setup Boilerplate

```tsx
import { ConfigProvider, theme, App as AntdApp } from 'antd';
import zhCN from 'antd/locale/zh_CN';

<ConfigProvider
  locale={zhCN}
  theme={{
    algorithm: isDark ? theme.darkAlgorithm : theme.defaultAlgorithm,
    token: {
      borderRadius: 10,
      colorPrimary: '#1677ff',
    },
  }}
>
  <AntdApp>
    <YourApp />
  </AntdApp>
</ConfigProvider>
```

`AntdApp` is required for static APIs (`message.success`, `Modal.info`, `notification`) to inherit theme/locale.
