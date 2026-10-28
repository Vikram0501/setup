# Deploy Selector

**Covers:** 60-second deploy decision + fallback ladder.
**Send when:** The app must run somewhere else, or you have > 15 min left at the end.
**Assumes:** `deployment-strategies.md` for the actual steps — this only picks the strategy number.

---

## The tree (stop at first yes)

1. Assessor will run it locally only? → **5. Repo quality only**
2. Prompt says Docker / containers? → **4. Docker → Fly.io** (Railway/Render also run Dockerfiles — use them if Fly signup stalls)
3. Next.js (stack A)? → **1. Vercel**
4. Static site or SPA build? → **2. Static host** (Netlify / GitHub Pages)
5. API-only / Node service? → **3. Railway or Render**
6. "Production-grade", "pipeline", "CI" also asked? → add **6. CI/CD** on top of 1–4

Default if truly unsure: frontend → Vercel; API-only → Render.

## Fallback ladder (when the first choice blocks)

| Blocked on | Fall back to |
|---|---|
| Vercel login / GitHub hookup fails | Render or Railway from the same repo |
| Fly.io CLI / auth friction | Railway/Render with the same Dockerfile |
| Free-tier Postgres signup fails | SQLite + seed script; declare "repo quality only" |
| Under 15 minutes left | Stop. Strategy 5 done properly beats strategy 1 half-done |

## Time budget

| Strategy | Allow |
|---|---|
| 2. Static host | 5 min |
| 1. Vercel | 10 min |
| 3. Railway / Render | 15 min |
| 4. Docker → Fly | 30 min |
| 6. CI/CD add-on | +20 min |

Hard rule: deploy work never starts before the vertical slice runs locally.
