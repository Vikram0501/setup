# Typography

**Covers:** Font pairings keyed to the palettes in `colour-schemes.md`, plus type scale, spacing and loading rules.
**Send when:** Any UI task where text is the main content — pair with `colour-schemes.md` and `ui-layout.md`.
**Assumes:** Tailwind or CSS variables. All fonts are free (Google Fonts / Fontsource). Max 2 families per project.

---

## Rules (apply to every pairing)

1. **Two families max** — one display (headings/UI) + one body. Hierarchy comes from weight and size, not more fonts.
2. **Body text ≥ 16px** (`text-base`). Nothing smaller than 12px anywhere.
3. **Line length 45–75 characters** — wrap prose in `max-w-prose` (~65ch). Tables and UI labels exempt.
4. **Line height:** display 1.1–1.2, headings 1.2–1.3, body 1.5–1.6. Larger text = tighter leading.
5. **Tracking:** large display headings `tracking-tight` (−0.02em); uppercase labels `tracking-wide`; body normal.
6. **Weights:** 400 body, 500–600 labels and headings, 700 sparingly. Max 3 weights loaded per family.
7. **Numbers in tables/dashboards:** `tabular-nums` so digits align.
8. **Load ≤ 2 families, latin subset, `display=swap`** — or use the system stack when speed beats polish.

---

## CSS variables boilerplate

```css
:root {
  --font-display: 'Sora', system-ui, sans-serif; /* headings + UI */
  --font-body: 'Inter', system-ui, sans-serif;   /* prose + labels */
  --font-mono: 'IBM Plex Mono', ui-monospace, monospace; /* code + data */
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

## Type scale

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

## Pairings (one per palette in colour-schemes.md)

| # | Palette | Display | Body | Mono | Why it works |
|---|---|---|---|---|---|
| 1 | Slate (neutral default) | Inter | Inter | JetBrains Mono | Zero-risk; everything reads as professional |
| 2 | Emerald (fintech) | Manrope | Manrope | JetBrains Mono | Geometric, confident; great tabular figures |
| 3 | Amber (commerce, warm) | Figtree | Figtree | Roboto Mono | Friendly roundness matches warm palettes |
| 4 | Rose (creative, bold) | Fraunces | Inter | — | Serif display + neutral body = editorial energy |
| 5 | Violet (AI, dev SaaS) | Sora | Inter | IBM Plex Mono | Modern geometric display; mono for code |
| 6 | Cyan (data, technical) | Space Grotesk | Inter | IBM Plex Mono | Squarish forms echo dashboards/charts |
| 7 | Monochrome (minimal/editorial) | Instrument Serif | Inter | — | One serif moment + everything else quiet |
| 8 | Tailwind default (speed) | system-ui | system-ui | ui-monospace | Zero load time — best under time pressure |

**Selection table** (same signals as `colour-schemes.md` — pick the row once, apply both docs):

| Prompt signals | Typography row |
|---|---|
| Nothing / unspecified | 1 (Inter) or 8 (system) |
| Finance, payments, analytics | 2 (Manrope) |
| Retail, food, lifestyle | 3 (Figtree) |
| Creative, portfolio, fashion | 4 (Fraunces + Inter) |
| AI, developer tool, startup SaaS | 5 (Sora + Inter) |
| Data viz, monitoring, technical | 6 (Space Grotesk + Inter) |
| Minimal, editorial, design-heavy | 7 (Instrument Serif + Inter) |
| Time pressure, design irrelevant | 8 (system stack) |

---

## Loading (pick one, in order of preference)

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

Never `@import` fonts in CSS (serialises loading). Never block render on font load — always `display=swap`.

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
