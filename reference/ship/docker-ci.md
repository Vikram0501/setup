# Docker & CI

**Covers:** Multi-stage Dockerfiles for the Node stacks and a GitHub Actions pipeline.
**Send when:** Prompt mentions Docker, containers, CI/CD or "production-grade"; also strategy 4/6 in `deployment-strategies.md`.
**Assumes:** Stacks A–C. Node 20 images (host Node version is irrelevant inside Docker).

---

## Dockerfile — API (stacks B / C)

```dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY package*.json ./
RUN npm ci --omit=dev
COPY --from=build /app/dist ./dist
USER node
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

## Dockerfile — Next.js (stack A)

Add `output: "standalone"` to `next.config.js`. Runtime stage copies `.next/standalone`, `.next/static` and `public`; run `node server.js` on `PORT`.

Lab machine stuck on Node 18? The container ships its own Node — host version doesn't matter. Running *without* Docker on Node 18: pin Next 14 / Vite 5.

## .dockerignore

```
node_modules
.next
dist
.git
.env
*.db
```

## docker-compose (app + Postgres, local parity)

```yaml
services:
  db:
    image: postgres:16-alpine
    environment: { POSTGRES_PASSWORD: postgres, POSTGRES_DB: app }
    ports: ["5432:5432"]
    volumes: [pgdata:/var/lib/postgresql/data]
  app:
    build: .
    depends_on: [db]
    environment: { DATABASE_URL: postgresql://postgres:postgres@db:5432/app }
    ports: ["3000:3000"]
volumes: { pgdata: {} }
```

## GitHub Actions

`.github/workflows/ci.yml` — lint, build, test on push + PR:

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

Deploy job: `if: github.ref == 'refs/heads/main'` + `npx vercel deploy --prod --token ${{ secrets.VERCEL_TOKEN }}` or `fly deploy --token ${{ secrets.FLY_API_TOKEN }}`. Secrets live in Settings → Secrets, never in YAML.

## Failure modes

| Symptom | Cause | Fix |
|---|---|---|
| `npm ci` fails in Docker | Lockfile missing or out of sync | Commit `package-lock.json`; regenerate with `npm install` |
| Huge image / OOM on free tier | Dev deps in final stage | Multi-stage + `--omit=dev` |
| Container exits instantly | Bound to localhost | Bind `0.0.0.0`, read `PORT` |
| Works in compose, not on host | Hardcoded `db` hostname | Use env-provided `DATABASE_URL` everywhere |
| CI green, prod broken | Migrations skipped | `prisma migrate deploy` in the build/release step |
