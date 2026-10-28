# Deployment Strategies

**Covers:** Deployment options, how to choose one, and step-by-step deploy sequences.
**Send when:** The task must run somewhere other than your machine — hosted demo, live URL, CI/CD, containerisation.
**Assumes:** JS/TS stacks from tech-stacks.md, but applies generally.

---

## Selection table

| Situation | Strategy |
|---|---|
| Next.js app, need a URL fast | **1. Vercel** |
| Static site or SPA | **2. Static host** (Netlify / GitHub Pages) |
| Node API / any server, free tier OK | **3. Railway or Render** |
| Must be a container / Docker asked for | **4. Docker → Fly.io** |
| Interviewers will run it locally | **5. Repo quality only** (no host) |
| Enterprise / "production-grade" asked | **6. CI/CD + staging → prod** |

**Default pick:** if unsure → Vercel for apps with a frontend, Render for API-only.

---

## The universal deploy checklist

Works for every strategy below:

1. **App boots from a clean clone** — new terminal, `npm ci && npm run build && npm start`.
2. **No secrets in the repo.** `.env.example` committed, `.env` in `.gitignore`.
3. **Database plan:** local dev = SQLite file. Hosted = hosted Postgres (Neon/Supabase free tier). SQLite + a read-only filesystem is the #1 deploy failure.
4. **Host environment variables** are set in the platform's dashboard — never shipped in code.
5. **Health check endpoint** (`/health` returning 200) if deploying a service.
6. **README** documents deploy steps so anyone can reproduce.

---

## 1. Vercel (fastest for Next.js)

```
Push to GitHub → vercel.com "Import Project" → Deploy → done
```

- Detects Next.js automatically; zero config.
- Database: add Neon/Supabase free tier, paste `DATABASE_URL` into Vercel env vars.
- CLI alternative: `npx vercel` from the project directory.
- **Time cost:** ~10 minutes including DB.

## 2. Static hosts (SPA / static site)

**Netlify:** connect repo → build command `npm run build` → publish dir `dist` (Vite) or `out` (Next export).
**GitHub Pages:** GitHub Action builds and pushes to `gh-pages` branch — use for pure static, no server logic.

- API needs? Point the SPA at a separate API host, or use Netlify/Vercel serverless functions to stay on one domain.

## 3. Railway / Render (Node API, general services)

| | Railway | Render |
|---|---|---|
| Free tier | Trial credit | Yes (with spin-down) |
| Setup | Repo → auto-detect | Repo → "Web Service" |
| Build | `npm ci && npm run build` | same |
| Start | `npm start` | same |
| DB | Add-on Postgres | Add-on Postgres |

Sequence:
1. Push repo to GitHub.
2. Create service from repo; set build/start commands.
3. Add managed Postgres; set `DATABASE_URL` env var.
4. Run migrations on deploy (`prisma migrate deploy` in build step or release phase).
5. Deploy. Verify `/health` and one real data path.

**Watch-outs:** free instances spin down (first request is slow — fine for demos); long builds hit limits.

## 4. Docker → Fly.io (containerisation)

**Dockerfile** (multi-stage, Node 20):

```dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY --from=build /app/dist ./dist
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

Sequence:
```bash
docker build -t app . && docker run -p 3000:3000 app   # verify locally first
fly launch   # interactive; generates fly.toml
fly deploy
```

Alternatives: **Railway/Render also run Dockerfiles** — if they can host it, skip Fly's setup.

## 5. Repo quality only (no live host needed)

When the assessor runs it locally, deployment means **frictionless reproduction**:

- `npm ci && npm run dev` works on a clean clone, no extra steps.
- Seed script: `npm run seed` creates demo data.
- README quickstart ≤ 10 lines.
- No absolute paths, no machine-specific config.
- Still commit a Dockerfile if the prompt mentions containers.

## 6. CI/CD (when "production-grade" or "pipeline" is asked)

GitHub Actions — build + test on every push, deploy on main:

```yaml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: npm }
      - run: npm ci
      - run: npm run lint
      - run: npm run build
      - run: npm test
```

Deploy job: add `if: github.ref == 'refs/heads/main'` with `vercel deploy --prod`, `fly deploy`, or `flyctl`/`render` CLI — pick the target from strategies 1–4.

**Staging → prod:** two environments (platform env vars), merge to `develop` → staging, tag/merge `main` → prod. Only include this if the prompt asks for it.

---

## Strategy comparison

| Strategy | Time | Cost | Needs | Looks most "production" |
|---|---|---|---|---|
| Vercel | ~10 min | Free | GitHub repo | ★★★ |
| Static host | ~5 min | Free | Built assets | ★ |
| Railway/Render | ~15 min | Free tier | GitHub repo | ★★★ |
| Docker → Fly | ~30 min | Free tier | Dockerfile | ★★★★ |
| Repo only | 0 min | — | — | ★ |
| CI/CD pipeline | +20 min | Free | Repo + platform account | ★★★★★ |

---

## Common failure modes

| Symptom | Cause | Fix |
|---|---|---|
| Works locally, 500s on host | SQLite on read-only FS | Use hosted Postgres |
| `EADDRINUSE` / crashes instantly | Bound to `localhost` | Bind `0.0.0.0`, read `PORT` env |
| Blank page after deploy | Absolute asset paths | Set relative/base URL config |
| Build OOMs on free tier | Dev deps in production build | Multi-stage Docker / `npm ci --omit=dev` |
| First request very slow | Free-tier cold spin | Acceptable for demos; note it in README |
| Works, then forgets data | Ephemeral container FS | Persist via DB service, not local files |

---

## Minimum viable answer

No time? Do this much, in order:
1. Push to GitHub with a working README.
2. Deploy with the default pick (Vercel / Render).
3. Put the live URL at the top of the README.
