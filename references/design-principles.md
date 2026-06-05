# Design Principles

The 15 universal design principles that every B2B / admin / dashboard prototype in this skill applies. They are not project-specific. Bake them into every new PRODUCT.md and DESIGN.md.

## 1. Information Density & Breathing Room

Balance data completeness with whitespace.

- Cards: 24px padding, 8px border-radius
- Inter-card gap: 24px (lg spacing)
- Page background: `#f5f5f5` (bottom layer)
- Cards: `#ffffff` + micro-shadow (middle layer)
- Buttons on cards: hover-lift (top layer)

**Rule of thumb**: 8px minimum between visual elements, 24px between cards, 32-48px between sections.

## 2. State as Language

Status is encoded by **color + icon + text** (triple encoding). Never color alone.

| State | Color | Icon | Example |
|---|---|---|---|
| Success | `#52c41a` | check | Healthy balance, growth |
| Warning | `#faad14` | warning | Low runway, anomaly |
| Error | `#f5222d` | close-circle | Out of balance, blocking error |
| Info | `#1677ff` | info | Neutral notice |

## 3. Operations Forward

High-frequency actions go in the most prominent position. Reduce click paths.

- "Recharge" button in alert banner AND in table header (two click paths)
- Theme toggle in top-right corner
- Search/filter near the data it filters

## 4. Zero-Friction Switching

Tenant / view / space switches must update data in place, not require manual refresh. Use `localStorage` to persist "last selected" per dimension.

## 5. Numeric Precision

- Use thousand separators: `4,500,000` (not `4500000`)
- Two decimal places for currency: `¥1,250.80`
- Signed percentages with direction: `+12.5%` / `-3.2%` / `0.0%`
- `font-variant-numeric: tabular-nums` for any column of numbers

## 6. The One Voice Rule

Primary blue `#1677ff` is reserved for: **primary actions, current selection, links**. Never decorative. Scarcity is credibility.

## 7. The Semantic State Rule

Success / Warning / Error colors are used only for their semantic states. Green never appears outside success scenarios.

## 8. No Glassmorphism

`backdrop-filter` and blur effects are forbidden. Shadow is the only depth tool. Stays crisp.

## 9. The Tight Scale Rule

Type scale ratio 1.125-1.2 (12 → 14 → 16 → 20 → 24). Exaggerated contrast creates noise. Tight scale keeps pages consistent.

## 10. The One Family Rule

Single font stack (system-ui). Weight (400/500/600) builds hierarchy, not family switching.

## 11. Subtle Layered Elevation

Always-on card shadow. Hover lifts. No transparent layers, no glow.

```
Card Rest:   0 1px 2px 0 rgba(0,0,0,0.03), 0 1px 6px -1px rgba(0,0,0,0.02), 0 2px 4px 0 rgba(0,0,0,0.02)
Card Hover:  0 4px 12px 0 rgba(0,0,0,0.08), 0 2px 8px -2px rgba(0,0,0,0.04)
Header:      0 1px 4px rgba(0,0,0,0.1)
```

## 12. No Marketing Speak

Forbidden words: streamline / empower / supercharge / leverage / unleash / transform / seamless / world-class / enterprise-grade / next-generation / cutting-edge / game-changer / mission-critical.

Use concrete nouns and verbs. Button labels are verb+object: "充值" not "立即体验".

## 13. CSS Grid + Flexbox

- CSS Grid for 2D (card grids, dashboards)
- Flexbox for 1D (button groups, nav bars)
- Never float, never table-for-layout

## 14. Semantic z-index

Never `999`. Use the standard scale:

```
dropdown:       1000
sticky:         1020
modal-backdrop: 1040
modal:          1050
toast:          1060
tooltip:        1070
```

## 15. Animation Discipline

- Only `transform` and `opacity`
- Never animate `width` / `height` / `padding` / `margin` (triggers layout)
- Always respect `prefers-reduced-motion`
- Duration 150-250ms, ease-out-quart

## 16. Don'ts (Hard Rules)

| Don't | Use Instead |
|---|---|
| `border-left` side-stripe | Full border, background, or icon |
| Gradient text / background-clip text | Solid color |
| Glassmorphism (blur, backdrop-filter) | Shadow |
| Big-number + tiny-label hero-metric | Number embedded in context |
| Identical card grids × N (icon + heading + text) | Mixed layouts, real data |
| Eyebrow all-caps labels ("DASHBOARD") | Section title in normal case |
| Em dashes (—) | Comma, colon, semicolon, parentheses |
| Marketing verb vocabulary | Concrete verb + object |
| Unselectable text in interactive elements | Inherit or `user-select: text` only on data |
| Multiple primary buttons next to each other | One primary + secondary buttons |
| Color-only state (e.g., red without icon/text) | Color + icon + text triple encoding |

## 17. Accessibility Baseline

- WCAG 2.1 AA: 4.5:1 body text, 3:1 large text
- Color blindness: state has icon + text fallback
- Keyboard: every interactive element reachable and operable
- `prefers-reduced-motion`: every animation has a fallback
- Screen reader: meaningful labels, not icon-only buttons (use `aria-label` + visible text or `Tooltip`)

## 18. Responsive Strategy

- Mobile-first considerations aren't required (most B2B dashboards are desktop)
- 1366 × 768 is the minimum supported viewport
- 1920 × 1080 is the design target
- Below 1024px: allow horizontal scroll, do not panic
- Above 1920px: max-width 1400px, centered

## 19. Code Quality (in implementation)

- Use antd `useToken()` for dynamic colors
- Never hardcode colors in JSX inline styles (use `token.colorXxx`)
- Use `ConfigProvider.useToken()` in functional components
- Use `theme.darkAlgorithm` / `theme.defaultAlgorithm` for theme switching
- Always wrap with `<App>` from antd for static API (`message`, `Modal`, `notification`)
- Persist user preferences (`theme`, `lastSelected`) via a `lib/persistence.ts` wrapper with SSR-safe fallbacks
