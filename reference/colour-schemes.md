# Colour Schemes

**Covers:** Ready-to-use palettes (light + dark) and rules for applying them.
**Send when:** Any task with a UI — web app, dashboard, landing page, form.
**Assumes:** Tailwind or plain CSS. Hex values map 1:1 to CSS variables.

---

## Rules (apply to every palette)

1. **60 / 30 / 10 split** — 60% background/surface, 30% text & borders, 10% accent.
2. **One accent colour only.** Use it for CTAs, links, active states. Never decorate with it.
3. **Ship dark mode if the app is developer-facing or used in low light** (dashboards, tools). Otherwise light only — less work.
4. **Never use pure `#000000` or `#FFFFFF` as background.** Use the palette's off-white / near-black to reduce glare.
5. **All text must pass WCAG AA**: 4.5:1 for normal text, 3:1 for large text. Accent-coloured text on white is the usual failure — check it.
6. **Semantic colours are separate from the palette**: success `#16A34A`, warning `#D97706`, error `#DC2626`, info `#2563EB`. Use these (or palette-adjacent variants) for toasts/validation — never the accent.

---

## CSS variables boilerplate

```css
:root {
  --bg: #FFFFFF;
  --surface: #F4F4F5;
  --border: #E4E4E7;
  --text: #18181B;
  --text-muted: #71717A;
  --accent: #2563EB;
  --accent-contrast: #FFFFFF;
}
[data-theme="dark"] {
  --bg: #09090B;
  --surface: #18181B;
  --border: #27272A;
  --text: #FAFAFA;
  --text-muted: #A1A1AA;
  --accent: #3B82F6;
  --accent-contrast: #FFFFFF;
}
```

---

## Palettes

### 1. Slate (neutral, safe default)

Professional, unopinionated. Use when the prompt says nothing about brand.

| Token | Light | Dark |
|---|---|---|
| bg | `#FFFFFF` | `#0B1120` |
| surface | `#F8FAFC` | `#1E293B` |
| border | `#E2E8F0` | `#334155` |
| text | `#0F172A` | `#F1F5F9` |
| text-muted | `#64748B` | `#94A3B8` |
| accent | `#2563EB` | `#3B82F6` |

### 2. Emerald (fintech, growth, success-oriented)

| Token | Light | Dark |
|---|---|---|
| bg | `#FFFFFF` | `#0A1410` |
| surface | `#F6FDF9` | `#12271E` |
| border | `#DCFCE7` | `#1E4D38` |
| text | `#052E16` | `#DCFCE7` |
| text-muted | `#4D7C5F` | `#86C7A5` |
| accent | `#059669` | `#34D399` |

### 3. Amber (warm, commerce, consumer-facing)

| Token | Light | Dark |
|---|---|---|
| bg | `#FFFBF5` | `#1A1207` |
| surface | `#FFF4E6` | `#2A1E0E` |
| border | `#FDE6C4` | `#4A3718` |
| text | `#1C1002` | `#FEF3E2` |
| text-muted | `#8A6A3B` | `#C4A275` |
| accent | `#D97706` | `#FBBF24` |

### 4. Rose (bold, marketing, creative)

| Token | Light | Dark |
|---|---|---|
| bg | `#FFFFFF` | `#190A10` |
| surface | `#FFF1F3` | `#2B121B` |
| border | `#FFE4E8` | `#52202F` |
| text | `#1C0A10` | `#FFE4E8` |
| text-muted | `#8C5666` | `#C88F9F` |
| accent | `#E11D48` | `#FB7185` |

### 5. Violet (AI, dev tools, modern SaaS)

| Token | Light | Dark |
|---|---|---|
| bg | `#FFFFFF` | `#0F0A1A` |
| surface | `#F7F5FF` | `#1B1430` |
| border | `#E9E4FF` | `#33285C` |
| text | `#140C24` | `#EDE9FE` |
| text-muted | `#6D5FA8` | `#A799D4` |
| accent | `#7C3AED` | `#A78BFA` |

### 6. Cyan (data, analytics, technical dashboards)

| Token | Light | Dark |
|---|---|---|
| bg | `#F9FEFF` | `#06131A` |
| surface | `#ECFBFF` | `#0C2733` |
| border | `#CFF3FF` | `#174A5E` |
| text | `#03141C` | `#E0F7FF` |
| text-muted | `#4E7E8E` | `#7FB4C7` |
| accent | `#0891B2` | `#22D3EE` |

### 7. Monochrome (minimal, portfolio, editorial)

| Token | Light | Dark |
|---|---|---|
| bg | `#FAFAFA` | `#0A0A0A` |
| surface | `#FFFFFF` | `#171717` |
| border | `#E5E5E5` | `#2A2A2A` |
| text | `#0A0A0A` | `#F5F5F5` |
| text-muted | `#737373` | `#A3A3A3` |
| accent | `#171717` | `#FAFAFA` |

> Note: monochrome accent = use border/weight/size for emphasis instead of colour.

### 8. Tailwind default (when speed beats design)

If design doesn't matter much, skip custom palettes entirely:

- Light: bg `#FFFFFF`, surface `#F9FAFB`, border `#E5E7EB`, text `#111827`, muted `#6B7280`, accent `#2563EB`
- Dark: bg `#111827`, surface `#1F2937`, border `#374151`, text `#F9FAFB`, muted `#9CA3AF`, accent `#3B82F6`

---

## Palette selection table

| Prompt signals | Palette |
|---|---|
| Nothing / unspecified | Slate (1) or Tailwind default (8) |
| Finance, payments, analytics | Emerald (2) |
| Retail, food, lifestyle | Amber (3) |
| Creative, portfolio, fashion | Rose (4) |
| AI, developer tool, startup SaaS | Violet (5) |
| Data viz, monitoring, technical | Cyan (6) |
| Minimal, editorial, design-heavy | Monochrome (7) |
| Time pressure, design irrelevant | Tailwind default (8) |
