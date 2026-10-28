# UI Layout & Usability

**Covers:** Layout, navigation, hierarchy and density rules that keep an interface clear, calm and fast to use.
**Send when:** Any UI task with more than one screen, a dashboard, or navigation — pair with `colour-schemes.md`.
**Assumes:** Tailwind or utility CSS; any component library (shadcn/ui etc.).

---

## The five clarity rules

1. **One primary action per view.** Exactly one button gets the primary/accent treatment; everything else is secondary or ghost.
2. **Three hierarchy levels max per screen.** Page title → section → content. Need a fourth? Split the page.
3. **Whitespace over borders.** Separate with space first; borders only for true boundaries (cards, tables, inputs).
4. **Consistency beats cleverness.** Same spacing, heading sizes and button placement on every screen.
5. **Content before chrome.** If an element doesn't answer "what can I do?" or "what does this say?" — cut it.

---

## Page archetypes

Pick one; don't invent a hybrid under time pressure.

| Archetype | Use for | Structure |
|---|---|---|
| App shell + sidebar | Dashboards, admin, multi-section tools | Fixed sidebar `w-64` + scrollable main, content `max-w-6xl mx-auto` |
| Top nav + single column | CRUD apps, simple SaaS ≤ 5 pages | Sticky header `h-14` + centered `max-w-5xl` |
| Split pane | Editors, chat, master-detail views | Left list (fixed width) + right detail (`flex-1 min-w-0`) |
| Single column form | Onboarding, surveys, checkout | `max-w-md mx-auto`, one concern per screen if long |
| Marketing / landing | Landing pages, portfolios | `max-w-6xl` stacked sections: hero → features → CTA → footer |

---

## Spacing

- One scale only: **4 / 8 / 12 / 16 / 24 / 32 / 48 / 64** (Tailwind `1 2 3 4 6 8 12 16`). Never invent in-between values.
- Section padding: `py-12`–`py-16`. Card padding: `p-4`–`p-6`. Related-item gaps: `gap-2`/`gap-4`. Between sections: `gap-8`+.
- Page gutters: `px-4` mobile, `px-6`/`px-8` desktop.
- If two elements feel crowded, **increase space before adding a divider**.

## Typography hierarchy

| Level | Size | Notes |
|---|---|---|
| Page title | `text-2xl`/`text-3xl` semibold | One per page |
| Section heading | `text-lg`/`text-xl` semibold | |
| Card / item title | `text-base` medium | |
| Body | `text-sm`/`text-base` | Secondary text uses muted colour |
| Caption / meta | `text-xs` | Timestamps, hints, counts |

- Max 2 font sizes per screen region. Never size by eye — use the scale.
- Left-align everything except empty states, auth pages and hero CTAs.

---

## Navigation

| Situation | Pattern |
|---|---|
| ≤ 5 top-level sections | Top nav bar, icon + label |
| > 5 sections or nested | Sidebar with collapsible groups |
| Both needed | Sidebar for sections; top bar for search / user / actions |

Rules:

- Active item: accent or `bg-muted` + font-weight or left bar — **never colour alone**.
- Labels ≤ 2 words, no jargon. Icons supplement labels; never replace them (except search, settings, close).
- Max 2 clicks to any core feature. Breadcrumbs only at depth ≥ 3.
- Header/sidebar sticky so navigation never scrolls away.

---

## Density by content type

| Element | Rules |
|---|---|
| Tables | Row `h-10`/`h-12`; header `bg-muted text-xs uppercase`; ≤ 7 columns then use a detail view; row actions right-aligned icon row |
| Cards | Equal height in grid (`items-stretch`); title + one metric/line + one action; ≤ 6 per section row |
| Lists | Avatar/icon + title + one meta line; row actions on hover (desktop), always visible (mobile) |
| Forms | One column (`max-w-md`); label above input; one primary submit at bottom; validate on blur, not per keystroke |
| Modals | Confirm / destructive / short edit only; `max-w-lg`; Escape + backdrop close; never nest modals |
| Dashboards | KPI row on top (3–4 stat cards) → main chart/table below → filters in one toolbar row, not scattered |

---

## Responsive

- Design at 1280px first, then fix 375px. **No horizontal scroll at any width.**
- Sidebar → hamburger or bottom nav under `md`. Tables → stacked cards under `sm`.
- Touch targets ≥ 44px on mobile.
- Three breakpoints (`sm`/`md`/`lg`) is enough — don't hand-build four.

---

## Clutter diagnosis

| Symptom | Cause | Fix |
|---|---|---|
| "Everything shouts" | Several primary buttons | One primary; rest secondary/ghost |
| Screen feels busy | Borders on every element | Drop dividers, use `gap` |
| Hard to scan | No consistent type scale | Apply the typography table above |
| Users get lost | > 7 nav items or no active state | Group/collapse; mark active clearly |
| Actions buried | Important action hidden in menus | Promote to page header |
| Cards look empty | Too much padding / too little content | `p-4`, cut to 2 lines, or merge cards |
| Endless scroll | Every concern on one page | Tabs or sections per concern |

---

## Usability checklist (before calling a UI done)

- [ ] One primary action stands out on every screen
- [ ] Active nav item clearly marked
- [ ] Same header / button placement on all pages
- [ ] No horizontal scroll at 375 / 768 / 1280px
- [ ] Every form field has a visible label
- [ ] Empty, loading and error states exist for data views
- [ ] Keyboard tab-through reaches all main actions; focus visible
- [ ] No text below `text-sm`; contrast verified per `colour-schemes.md`
