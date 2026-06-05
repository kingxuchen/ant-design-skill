# Discovery Questions for New Product

When the user invokes this skill, ask these questions in 2-3 rounds to gather enough information to generate PRODUCT.md and DESIGN.md. Don't ask all at once — group them and follow up.

## Round 1: Core Identity (must have)

These 4 questions are required to generate a usable PRODUCT.md. Ask them first, all together.

1. **产品名称 / Project name** — what should we call this product? (one-liner name)
2. **产品定位 / What is it?** — one sentence: "[For whom] [doing what] [in what context]"?
3. **主要使用场景 / Top 3 use cases** — list the 3 most important user journeys
4. **目标用户 / Target users** — who uses it daily? What's their technical background?

## Round 2: Brand & Feel (should have)

These shape DESIGN.md. Ask after Round 1 is answered.

5. **品牌气质 / Brand personality** — pick 2-3 adjectives from: 现代友好 / 可信赖 / 高效 / 简洁 / 严肃专业 / 趣味 / 极客 (or describe your own)
6. **参考产品 / Reference products** — what existing products' UI do you admire? (e.g., "Linear + Notion", "Stripe Dashboard", "Vercel")
7. **主色 / Primary color** — do you have a brand color? (hex code or describe; default to `#1677ff` if unsure)
8. **关键禁忌 / Anti-references** — what should we definitely NOT look like? (e.g., "not Slack", "not Bootstrap admin")

## Round 3: Feature & Scope (nice to have)

Ask only if not clear from Round 1-2, or if the user is open to scope discussion.

9. **核心功能 / Core features** — what's the minimum page set? (e.g., "login + dashboard + list + detail")
10. **数据来源 / Data source** — is this purely mock data, or is there a real API to integrate?
11. **时间线 / Timeline** — is this a quick demo (1 page) or fuller prototype (3-5 pages)?
12. **多租户? / Multi-tenant?** — does the data have a tenant/company dimension? (affects whether to add a tenant switcher)

## What to Skip Asking

Don't ask about:
- Specific component preferences (the skill knows what to use)
- Exact spacing/colors (the skill has defaults from DESIGN.md)
- Tech stack details (locked: vite + react + antd + typescript)
- File structure (boilerplate handles it)

If the user provides vague answers, make reasonable assumptions and state them in the generated PRODUCT.md under "Working Assumptions".

## What to Do If User Just Says "Build Me a Dashboard"

If the user provides no specifics, do this:
1. State 2-3 sensible assumptions (e.g., "B2B admin dashboard, financial + usage monitoring, mock data, desktop-first")
2. Generate PRODUCT.md with placeholder sections clearly marked
3. Build a representative single-page prototype (e.g., a financial overview dashboard)
4. Ask the user to refine after seeing the result

The goal is to ship something visible quickly, not to interrogate.
