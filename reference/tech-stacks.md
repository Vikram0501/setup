# Tech Stacks

**Covers:** Curated, default tech stacks per task type. Pick one and go.
**Send when:** Starting any build task; the prompt doesn't mandate a stack.
**Assumes:** JavaScript/TypeScript. Node 20+. No stack here requires paid services.

---

## Golden rules

1. **Never choose a stack the prompt didn't ask for** — if the prompt names tools, use them.
2. **Pick from this doc in under 60 seconds.** Don't evaluate alternatives mid-assessment.
3. **Prefer boring and known** over new and interesting. Assessments reward working software.
4. **Default to TypeScript + Tailwind + SQLite** unless the task says otherwise.
5. **One database, no message queues, no microservices** unless explicitly required.

---

## Selection table

| Prompt signals | Stack |
|---|---|
| "web app", "full-stack", "CRUD", most default asks | **A — Next.js Full-Stack** |
| "dashboard", "SPA", "single page", separate API wanted | **B — Vite SPA + API** |
| "REST API", "backend", "service", "endpoint" | **C — Node API** |
| "CLI", "script", "automation", "tool" | **D — CLI/Script** |
| "data", "CSV", "JSON processing", "ETL" | **E — Data task** |
| "real-time", "chat", "live updates" | **F — Real-time** |
| Static site, docs, portfolio, "no backend" | **G — Static site** |

---

## A — Next.js Full-Stack (default choice)

| Layer | Choice |
|---|---|
| Framework | Next.js 15 (App Router) + TypeScript |
| Styling | Tailwind CSS + shadcn/ui components |
| Data | Prisma + SQLite (local) / Postgres (deployed) |
| Auth | NextAuth (Auth.js) — only if prompt requires auth |
| Validation | Zod |
| State | React state first; Zustand only if genuinely complex |
| Deploy | Vercel (or Docker → any host) |

```bash
npx create-next-app@latest app --typescript --tailwind --eslint --app
npx shadcn@latest init
npm i prisma @prisma/client zod
npx prisma init --datasource-provider sqlite
```

**Use when:** task needs UI + data + logic in one artifact. Fastest path to a demoable app.
**Skip when:** prompt explicitly wants a separate backend.

## B — Vite SPA + Node API

| Layer | Choice |
|---|---|
| Frontend | Vite + React + TypeScript + Tailwind |
| Backend | Express (or Hono for smaller surface) |
| Data | SQLite via better-sqlite3 or Prisma |
| Shared | Single repo, `/client` + `/server`, shared `/shared` types |
| Deploy | Frontend: static host. API: Railway/Render/Fly |

```bash
npm create vite@latest client -- --template react-ts
npm create vite@latest server -- --template vanilla
cd server && npm i express cors zod better-sqlite3
```

**Use when:** prompt wants a clean client/server split, or asks for an SPA.
**Skip when:** a simpler all-in-one will do (choose A).

## C — Node API

| Layer | Choice |
|---|---|
| Runtime | Node 20 + TypeScript (tsx for dev) |
| Framework | Hono (tiny, fast) or Express (familiar) |
| Validation | Zod at every route boundary |
| Data | SQLite (better-sqlite3 / Drizzle) — Postgres if stated |
| Tests | Vitest + supertest |
| Deploy | Docker → Fly.io / Railway / Render |

```bash
mkdir api && cd api && npm init -y
npm i hono @hono/node-server zod drizzle-orm better-sqlite3
npm i -D typescript tsx vitest @types/node
```

**Use when:** "API", "backend", "service", webhook receivers, anything headless.
**Skip when:** UI is required (choose A or B).

## D — CLI / Script

| Layer | Choice |
|---|---|
| Runtime | Node + TypeScript |
| Runner | `tsx` for dev, `esbuild` bundle for ship |
| Args | Commander (complex) or plain `process.argv` (simple) |
| Output | chalk for colour, ora for spinners — only if interactive |
| Package | `"type": "module"`, `bin` field for installable CLIs |

```bash
npm init -y && npm i -D typescript tsx @types/node
npm i commander
```

**Use when:** automation, file processing, anything run in a terminal.
**Rule:** no framework until you feel pain. A 100-line script doesn't need architecture.

## E — Data task

| Layer | Choice |
|---|---|
| Runtime | Node + TypeScript |
| Parsing | csv-parse, papaparse, or native JSON |
| Compute | Plain JS. Pandas-class need → use Python, it's allowed |
| Output | JSON/CSV files + a small summary report |
| Viz | None unless asked; if asked: Chart.js or Recharts |

**Use when:** transform/analyse/generate data.
**Rule:** print intermediate results constantly; silent scripts waste assessment time.

## F — Real-time

| Layer | Choice |
|---|---|
| Base | Stack A (Next.js) or C (Node API) |
| Transport | WebSocket via `ws` (server) + native `WebSocket` (client) |
| Alternative | Server-Sent Events for one-way feeds — 3x less code |
| State | Keep messages in memory; add Redis only if multi-instance |

**Use when:** chat, live dashboards, notifications.
**Rule:** SSE over WebSockets unless the prompt demands bidirectional.

## G — Static site

| Layer | Choice |
|---|---|
| Builder | Next.js output:"export", Astro, or plain HTML/CSS/JS |
| Content | Markdown + a content collection if content-heavy |
| Deploy | Vercel / Netlify / GitHub Pages |
| Forms | Formspree or a single serverless function if needed |

**Use when:** landing pages, portfolios, docs, "no backend required".

---

## Deliberate exclusions (don't reach for these under time pressure)

| Avoid | Instead |
|---|---|
| MongoDB | SQLite/Postgres — schemas beat flexibility in assessments |
| Redux | React state, Zustand if needed |
| GraphQL | REST — one mental model, faster |
| gRPC / microservices | One process |
| CSS-in-JS (Styled-components) | Tailwind |
| Webpack from scratch | Vite / framework defaults |
| Kubernetes / ECS | Platform-as-a-service (see deployment-strategies.md) |

---

## Universal invariants (every stack above)

- TypeScript with `strict: true`
- ESLint + Prettier (or Biome — one tool, both jobs)
- Zod (or equivalent) validating all external input
- `.env.example` committed; `.env` git-ignored
- `npm run build` must pass before you call anything "done"
- README with: setup, run, build, test commands
