# UI Style (Colour + Type)

**Covers:** Palettes (light + dark), font pairings keyed to those palettes, type scale, spacing/loading rules.
**Send when:** Any task with a UI. Attach with `ui-layout.md` for multi-screen builds.
**Assumes:** Tailwind or plain CSS. Hex values map 1:1 to CSS variables. All fonts free (Google Fonts / Fontsource). Max 2 families.

---

## Rules (apply to every UI)

1. **60 / 30 / 10 split** — 60% background/surface, 30% text & borders, 10% accent.
2. **One accent colour only.** CTAs, links, active states. Never decorate with it.
3. **Never pure `#000000` / `#FFFFFF` as background** — use the palette's off-white / near-black.
4. **All text passes WCAG AA**: 4.5:1 normal, 3:1 large. Accent-coloured text on white is the usual failure.
5. **Semantic colours separate from the palette**: success `#16A34A`, warning `#D97706`, error `#DC2626`, info `#2563EB` — never the accent.
6. **Two font families max** — one display (headings/UI) + one body. Hierarchy from weight and size, not more fonts.
7. **Body text ≥ 16px** (`text-base`); nothing smaller than 12px. Line length 45–75ch (`max-w-prose`).
8. **Line height:** display 1.1–1.2, headings 1.2–1.3, body 1.5–1.6. **Tracking:** large display `tracking-tight`, uppercase labels `tracking-wide`.
9. **Weights:** 400 body, 500–600 labels/headings, 700 sparingly. Max 3 loaded per family. Tables/dashboards: `tabular-nums`.
10. **Ship dark mode** if developer-facing or low-light use (dashboards, tools). Otherwise light only — less work.

---

## CSS variables boilerplate

```css
:root {
  --bg: #FFFFFF;
  --surface: #F8FAFC;
  --border: #E2E8F0;
  --text: #0F172A;
  --text-muted: #64748B;
  --accent: #2563EB;
  --accent-contrast: #FFFFFF;
  --font-display: 'Inter', system-ui, sans-serif;
  --font-body: 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', ui-monospace, monospace;
}
[data-theme="dark"] {
  --bg: #0B1120;
  --surface: #1E293B;
  --border: #334155;
  --text: #F1F5F9;
  --text-muted: #94A3B8;
  --accent: #3B82F6;
  --accent-contrast: #FFFFFF;
}
```

Tailwind (`tailwind.config.ts`):

```ts
fontFamily: {
  display: ['var(--font-display)', 'system-ui', 'sans-serif'],
  body: ['var(--font-body)', 'system-ui', 'sans-serif'],
  mono: ['var(--font-mono)', 'ui-monospace', 'monospace'],
}
```

---

## Palettes (rows pair with the font column)

| # | Use when (prompt signals) | bg | surface | border | text | text-muted | accent | Fonts (display / body / mono) |
|---|---|---|---|---|---|---|---|---|
| 1 | Nothing specified — safe default (Slate) | `#FFFFFF` / `#0B1120` | `#F8FAFC` / `#1E293B` | `#E2E8F0` / `#334155` | `#0F172A` / `#F1F5F9` | `#64748B` / `#94A3B8` | `#2563EB` / `#3B82F6` | Inter / Inter / JetBrains Mono |
| 2 | Finance, payments, analytics (Emerald) | `#FFFFFF` / `#0A1410` | `#F6FDF9` / `#12271E` | `#DCFCE7` / `#1E4D38` | `#052E16` / `#DCFCE7` | `#4D7C5F` / `#86C7A5` | `#059669` / `#34D399` | Manrope / Manrope / JetBrains Mono |
| 3 | Retail, food, lifestyle (Amber) | `#FFFBF5` / `#1A1207` | `#FFF4E6` / `#2A1E0E` | `#FDE6C4` / `#4A3718` | `#1C1002` / `#FEF3E2` | `#8A6A3B` / `#C4A275` | `#D97706` / `#FBBF24` | Figtree / Figtree / Roboto Mono |
| 4 | Creative, portfolio, fashion (Rose) | `#FFFFFF` / `#190A10` | `#FFF1F3` / `#2B121B` | `#FFE4E8` / `#52202F` | `#1C0A10` / `#FFE4E8` | `#8C5666` / `#C88F9F` | `#E11D48` / `#FB7185` | Fraunces / Inter / — |
| 5 | AI, dev tools, startup SaaS (Violet) | `#FFFFFF` / `#0F0A1A` | `#F7F5FF` / `#1B1430` | `#E9E4FF` / `#33285C` | `#140C24` / `#EDE9FE` | `#6D5FA8` / `#A799D4` | `#7C3AED` / `#A78BFA` | Sora / Inter / IBM Plex Mono |
| 6 | Data viz, monitoring, technical (Cyan) | `#F9FEFF` / `#06131A` | `#ECFBFF` / `#0C2733` | `#CFF3FF` / `#174A5E` | `#03141C` / `#E0F7FF` | `#4E7E8E` / `#7FB4C7` | `#0891B2` / `#22D3EE` | Space Grotesk / Inter / IBM Plex Mono |
| 7 | Minimal, editorial, design-heavy (Mono) | `#FAFAFA` / `#0A0A0A` | `#FFFFFF` / `#171717` | `#E5E5E5` / `#2A2A2A` | `#0A0A0A` / `#F5F5F5` | `#737373` / `#A3A3A3` | `#171717` / `#FAFAFA` | Instrument Serif / Inter / — |
| 8 | Time pressure, design irrelevant (Tailwind default) | `#FFFFFF` / `#111827` | `#F9FAFB` / `#1F2937` | `#E5E7EB` / `#374151` | `#111827` / `#F9FAFB` | `#6B7280` / `#9CA3AF` | `#2563EB` / `#3B82F6` | system-ui / system-ui / ui-monospace |

Each row: light value / dark value. Row 7 accent = use border/weight/size for emphasis instead of colour. Row 8 = zero load time — best under time pressure.

---

## Type scale (row 8 = system stack, no scale decisions needed)

| Role | Tailwind | Size / line-height | Notes |
|---|---|---|---|
| Meta / caption | `text-xs` | 12 / 16 | Timestamps, hints |
| Secondary | `text-sm` | 14 / 20 | Table cells, descriptions |
| Body | `text-base` | 16 / 24 | Default — never go below this |
| Lead | `text-lg` | 18 / 28 | Intro paragraphs |
| Card title | `text-xl` | 20 / 28 | |
| Page title | `text-2xl` | 24 / 32 | One per page |
| Section | `text-3xl` | 30 / 36 | |
| Hero | `text-4xl`–`text-5xl` | 36–48 / tight | Landing pages only |
| Display | `text-6xl`+ | 48+ / 1.05 | Marketing heroes only |

Don't size by eye — pick the nearest step and move on.

---

## Font loading (pick one, in order of preference)

**1. Fontsource (npm) — preferred on restricted/lab networks**

```bash
npm i @fontsource-variable/inter @fontsource-variable/sora
```

```ts
import '@fontsource-variable/inter'
import '@fontsource-variable/sora'
```

**2. Google Fonts link — fastest to wire up**

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Sora:wght@600;700&display=swap" rel="stylesheet">
```

**3. System stack — no network dependency, zero CLS**

```css
--font-body: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
```

Never `@import` fonts in CSS (serialises loading). Always `display=swap`.

---

## Common failures

| Symptom | Cause | Fix |
|---|---|---|
| Text feels tiny/cramped | Body set to 14px | `text-base` minimum, lh 1.5 |
| Wall of text | Lines > 80 chars | `max-w-prose` |
| Flash of invisible text | No `display=swap` | Add swap + preconnect |
| Fonts never load (lab net) | CDN blocked | Switch to Fontsource or system stack |
| Jittering digits in tables | Proportional numbers | `tabular-nums` |
| Headings feel flat | One weight everywhere | 600 headings, 400 body, tight tracking |
| Too many fonts | 3+ families | Delete one; use weight for hierarchy |
| Accent text unreadable | Accent on white bg | Darken accent or use as background only |
