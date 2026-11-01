# Entry Point (START HERE)

**Covers:** The contract for a full task run — routing to the other setup docs, mandatory pause points, commit cadence.
**Send when:** Always — the single doc you attach (with the spec PDF). The agent reads further docs from this folder only as routed below.
**Assumes:** This setup repo is readable by the agent. Spec arrives as a PDF or long brief.

---

## The contract

1. **Read this file first, then only what the routing table selects.** Never read the whole folder — unread docs cost nothing; over-reading wastes the budget. Typical run: this → `spec-to-plan.md` → 2–3 topical docs.
2. **Plan first, pause for confirmation.** Extract requirements and emit the plan in the `spec-to-plan.md` format, then **stop and wait**. Building starts only after the user's go-ahead.
3. **Pause at the checkpoints below.** Between checkpoints work fully autonomously — no questions, no permission-seeking, no narration. At each checkpoint: terse status + proof + next steps, then wait.
4. **Commit and push automatically** per `git-methodology.md` at every green milestone — never ask, never wait for permission on commits.
5. **Never ask clarifying questions** — make assumptions and list them. Pauses are scheduled reviews, not Q&A.

## Checkpoints (in order)

| # | When | Report | Then |
|---|---|---|---|
| 1 | Plan ready | Plan per `spec-to-plan.md` format + assumptions | **Stop. Wait for confirmation** |
| 2 | Vertical slice demoable (one entity end-to-end) | What works + the exact commands that prove it | Stop, wait for "continue" |
| 3 | Each feature batch after that; also when blocked (same bug 20 min, tooling broken 10 min) | Delta: what landed + verified; what's next; cuts proposed | Stop, wait for "continue" |
| 4 | Feature-complete / pre-deploy | Full `definition-of-done.md` run results | Stop; deploy only on approval |

User replies "continue" → proceed unchanged. User cuts scope → record cuts in README "Known gaps", adapt immediately, move on. No pushing back on cuts.

## Routing table (signal → read next)

| Spec says | Read |
|---|---|
| Always (PDF / long brief) | `spec-to-plan.md` — extraction, classification, plan format |
| Always | `git-methodology.md` — commit/push cadence |
| Always (checkpoint 4) | `definition-of-done.md` |
| Any build | `tech-stacks.md` — pick one stack in ≤60s; prompt-named tools always win |
| Any build | `project-scaffolds.md` — folder skeleton + first-commit check |
| Has a UI | `ui-style.md` — one palette/font row from its selection table |
| Multi-screen, dashboard, navigation | `ui-layout.md` |
| Backend / endpoints | `api-and-data-conventions.md` |
| Login, accounts, roles | `auth.md` |
| Tests requested (or REVIEW) | `testing.md` |
| Must run elsewhere / live URL | `deployment-strategies.md` |
| Docker / containers / CI | `docker-ci.md` + `deployment-strategies.md` |

Skip anything the spec doesn't imply. Never re-read or re-paste a doc already in context — refer to it by filename.

## Output style

Terse everywhere. At each checkpoint ≤30 lines: status → proof (commands actually run) → new assumptions/risks → next steps. No code explanations, no restating files, no plans for work not yet approved.
