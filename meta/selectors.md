# Selectors (Stack + Deploy)

**Covers:** Two 60-second decision trees — stack letter (A–G) and deploy strategy (1–6).
**Send when:** Never attach — this is for you. Stack tree before you build; deploy tree when the app must run elsewhere or you have > 15 min left.
**Assumes:** `tech-stacks.md` and `deployment-strategies.md` for the actual steps — these only pick the letter/number.

---

## Stack tree (answer in order, stop at first yes)

1. Prompt demands a specific stack? → **use it. Tree over.** (Never override the prompt.)
2. Real-time bidirectional (chat, live collaboration)? → **F**
3. UI required?
   - One deployable app, no explicit client/server split → **A** (default)
   - Explicit SPA + separate API, or "decoupled" → **B**
   - Static only / "no backend" → **G**
4. Headless (API, webhook, service)? → **C**
5. Terminal (CLI, script, automation)? → **D**
6. Data transform / analysis (CSV, JSON, ETL)? → **E**
7. Still unsure? → **A.** It's the default for a reason.

### Stack lock-in rules

| Rule | Why |
|---|---|
| Decide by minute 5, never revisit | Mid-task stack switches lose the assessment |
| One database, one process | Microservices and queues fail demos |
| Prompt-named tools beat this tree | Golden rule 1 in `tech-stacks.md` |
| Lab machine on Node 18? A→Next 14, B→Vite 5 | Newer majors need Node 20+ |

---

## Deploy tree (stop at first yes)

1. Assessor will run it locally only? → **5. Repo quality only**
2. Prompt says Docker / containers? → **4. Docker → Fly.io** (Railway/Render also run Dockerfiles — use them if Fly signup stalls)
3. Next.js (stack A)? → **1. Vercel**
4. Static site or SPA build? → **2. Static host** (Netlify / GitHub Pages)
5. API-only / Node service? → **3. Railway or Render**
6. "Production-grade", "pipeline", "CI" also asked? → add **6. CI/CD** on top of 1–4

Default if truly unsure: frontend → Vercel; API-only → Render.

### Fallback ladder (when the first choice blocks)

| Blocked on | Fall back to |
|---|---|
| Vercel login / GitHub hookup fails | Render or Railway from the same repo |
| Fly.io CLI / auth friction | Railway/Render with the same Dockerfile |
| Free-tier Postgres signup fails | SQLite + seed script; declare "repo quality only" |
| Under 15 minutes left | Stop. Strategy 5 done properly beats strategy 1 half-done |

### Deploy time budget

| Strategy | Allow |
|---|---|
| 2. Static host | 5 min |
| 1. Vercel | 10 min |
| 3. Railway / Render | 15 min |
| 4. Docker → Fly | 30 min |
| 6. CI/CD add-on | +20 min |

Hard rule: deploy work never starts before the vertical slice runs locally.
