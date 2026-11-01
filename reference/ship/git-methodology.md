# Git Methodology

**Covers:** Commit/push cadence, message format, secrets hygiene and recovery — how the agent ships version control automatically during a build.
**Send when:** Any BUILD or DEPLOY task. Small (~1KB) — safe default attach.
**Assumes:** Repo has a remote on `main`. JS/TS stack.

---

## Rules

1. **Work on `main` directly.** No feature branches — branch/merge overhead loses timed assessments.
2. **Commit at every green milestone:** scaffold builds · vertical slice works · each feature lands · pre-deploy. Never go > 20 min without a commit.
3. **Never commit a broken build.** `npm run build` (and `npm test` if present) must pass before every commit.
4. **Push after every commit** — `git push origin main`. The remote is the backup; a local-only repo dies with the machine.
5. **Messages:** one line, conventional, imperative — `feat: task CRUD API` · `fix: auth redirect loop` · `chore: scaffold Next.js app` · `docs: README quickstart`.
6. **Never commit:** `.env`, `node_modules`, `dist`, `*.db`, IDE configs. `.env.example` yes. Verify with `git status` before each commit — stage intentionally (`git add <paths>`), never `git add .` blind.
7. **Fix forward, never rewrite.** Broken commit already pushed → fix, verify, new commit, push. No force-push, no history edits.
8. **First commit:** `git init` + remote if needed, then skeleton + green build. Nothing half-finished.

## Milestone cadence

| Just finished | Commit |
|---|---|
| Scaffold + green build | `chore: scaffold <stack> with green build` |
| Vertical slice (entity end-to-end) | `feat: <entity> end-to-end (UI → API → DB)` |
| Each secondary feature | `feat: <feature>` |
| Test suite added/passing | `test: <what>` |
| Pre-deploy / DoD pass | `chore: pre-submit — DoD pass` |

## Recovery

| Situation | Do |
|---|---|
| Push rejected (remote ahead) | `git pull --rebase origin main` then push |
| Build broke after commit | Fix → verify → `git commit -m "fix: ..."` → push |
| `git add .` staged junk | `git restore --staged <paths>` — never commit it |

## Done means pushed

Final sequence: DoD pass → final commit → `git push` → confirm the remote URL loads in a browser. A perfect local repo with an unpushed final commit is not submitted.
