# Project Scaffolds

**Covers:** Exact folder layout, npm scripts and first-commit contents for stacks A–G in `tech-stacks.md`.
**Send when:** Starting a build; you want the skeleton decided so feature work starts immediately.
**Assumes:** Node 20+ (Node 18 pins noted). Stack letter already picked (`meta/selectors.md`).

---

## Universal skeleton (every stack)

```
<app>/
├── src/              # all source; no loose files at root
├── .env.example      # committed; every var documented
├── .gitignore        # node_modules, .env, dist, *.db
├── package.json
├── tsconfig.json     # strict: true
└── README.md         # setup / run / build / test in ≤ 10 lines
```

Every project ships these scripts from minute one:

```json
"scripts": {
  "dev": "...",
  "build": "...",
  "start": "...",
  "lint": "...",
  "test": "vitest run"
}
```

First commit = universal skeleton + stack scaffold below + green `npm run build`. Nothing else.

---

## A — Next.js full-stack

```
app/
├── (marketing)/page.tsx        # public pages, optional
├── (dashboard)/
│   ├── layout.tsx              # sidebar/app shell
│   └── <entity>/page.tsx       # list; [id]/page.tsx = detail
├── api/<entity>/route.ts       # Route Handlers (or server actions)
├── layout.tsx
└── globals.css
prisma/
├── schema.prisma
└── seed.ts
src/
├── components/ui/              # shadcn
├── components/<entity>/
└── lib/{db.ts,auth.ts,utils.ts}
```

- `npx prisma migrate dev --name init` immediately after schema — commit the migration.
- Seed: `npm i -D tsx` + `"prisma": { "seed": "tsx prisma/seed.ts" }` in package.json; expose `npm run seed`.
- First-commit check: `npx prisma migrate dev && npm run seed && npm run build`.

## B — Vite SPA + API

```
client/           # Vite react-ts
└── src/{pages,components,api/client.ts,lib}
server/           # vanilla node + Express/Hono
└── src/{index.ts,routes/,db.ts,zod-mw.ts}
shared/           # optional: types imported by both
```

- Vite dev proxy: `server: { proxy: { "/api": "http://localhost:4000" } }` — same-origin in dev, no CORS pain.
- Two package.jsons; root README has one run line per side. First-commit check: build both sides.

## C — Node API

```
api/src/
├── index.ts          # app.listen(PORT ?? 3000, "0.0.0.0")
├── routes/<entity>.ts
├── middleware/{auth.ts,errors.ts}
└── db.ts
```

- Add `GET /health` → 200 from minute one (deploy + CI depend on it).
- First-commit check: `npm run build && node dist/index.js` boots and `/health` responds.

## D — CLI / script

```
src/
├── cli.ts            # bin entry, shebang, commander
├── commands/
└── lib/
```

- `"type": "module"`, `"bin": { "<name>": "./dist/cli.js" }`.
- First-commit check: `npx tsx src/cli.ts --help` works; input/output paths relative to cwd, never absolute.

## E — Data task

```
src/{ingest.ts,transform.ts,report.ts}
data/input/          # committed sample
data/output/         # git-ignored
```

- Print progress every step; ship a sample input so the assessor can rerun instantly.
- First-commit check: run on the sample, output file + summary printed.

## F — Real-time

- Start from stack A or C. Add `src/lib/rooms.ts` — in-memory `Map`. No Redis.
- SSE endpoint `/events` (one-way) unless bidirectional is required.
- First-commit check: two browser tabs see a message from one tab.

## G — Static site

```
index.html
styles.css
script.js            # or assets/ bundle
assets/
```

- Plain HTML/CSS/JS unless a builder earns its keep. One `npm run dev` via `npx serve`.
- First-commit check: open `index.html` via the serve script — no build step to forget.
