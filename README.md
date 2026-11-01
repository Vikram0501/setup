# Setup Docs

Repo of docs an AI coding agent (Qoder) reads during timed build assessments. One prompt + the spec PDF in, staged implementation out — the agent routes itself to the docs it needs and pauses for your review at fixed checkpoints.

## How to use

1. Download the spec PDF. Put this repo somewhere the agent can read it (e.g. clone into the project as `./setup/`).
2. Copy the **BUILD** prompt from `meta/prompt-templates.md`; fill in TASK (prompt verbatim), SPEC (PDF path), SETUP DOCS (path to `reference/entry.md`), and TIME if known.
3. The agent reads `entry.md`, extracts the plan per `spec-to-plan.md`, and **pauses for your confirmation** (checkpoint 1). Review: reply "continue", or cut/change anything — cuts win.
4. It then builds, pausing at each checkpoint (vertical slice, feature batches, pre-deploy) with proof of what works. Commits and pushes happen automatically throughout (`git-methodology.md`).
5. Final pass: **REVIEW** prompt + `definition-of-done.md`. Broken bits → **FIX** prompt; hosting → **DEPLOY** prompt.
6. Optional: pre-pick stack/deploy yourself via `meta/selectors.md` and phases via `meta/time-budget.md` — or approve the agent's choices at checkpoint 1.

One-time setup (rules + `/build` command) is in `meta/prompt-templates.md`.

## Doc index

**Meta — for you, never send to the agent:**

| Doc | Send when |
|---|---|
| `meta/prompt-templates.md` | Always — the prompts to send + Qoder setup |
| `meta/selectors.md` | You want to pick stack/deploy yourself, fast |
| `meta/time-budget.md` | Task released — allocate the hours + kill criteria |

**Reference — read by the agent (self-contained, `Covers / Send when / Assumes` header):**

| Doc | Read when |
|---|---|
| `reference/entry.md` | **Always — the only doc you send.** Routes the run: checkpoints + which docs to read |
| `reference/spec-to-plan.md` | Spec is a PDF / long brief — extraction + plan format (checkpoint 1) |
| `reference/build/tech-stacks.md` | Any build; stack not dictated by the prompt |
| `reference/build/project-scaffolds.md` | Any build — exact skeleton + first-commit check |
| `reference/build/api-and-data-conventions.md` | Backend / endpoints |
| `reference/ui/ui-style.md` | Task has a UI — palettes + fonts + type scale, one row per signal |
| `reference/ui/ui-layout.md` | Multi-screen, dashboard, navigation |
| `reference/features/auth.md` | Login, accounts or roles mentioned |
| `reference/features/testing.md` | Tests requested; or REVIEW needs proof |
| `reference/ship/deployment-strategies.md` | Must run somewhere other than your machine |
| `reference/ship/docker-ci.md` | Docker, containers, CI/pipeline (pair with deployment-strategies) |
| `reference/ship/git-methodology.md` | Always — commit/push cadence |
| `reference/ship/definition-of-done.md` | Final checkpoint / REVIEW |

## Which docs for which prompt

| Prompt signals | Prompt | Send |
|---|---|---|
| Any build / spec PDF | BUILD | `entry.md` + the PDF (agent routes to everything else itself) |
| "Fix", "not working", error output | FIX | (none) |
| "Deploy", "host", "live URL" | DEPLOY | deployment-strategies (+ selectors for you) |
| "Review", "verify", "check my solution" | REVIEW | definition-of-done (+ testing if tests were asked; + tech-stacks if stack compliance matters) |

Rule of thumb: **one doc per send** — `entry.md` for builds, nothing for fixes, the named doc for deploy/review. The agent only reads routed docs that the spec implies (unread docs cost 0 tokens); never attach extras "just in case".

## Conventions

Docs refer to each other by bare filename only. Folder rule of thumb: `build/` = how the code is structured, `ui/` = how it looks, `features/` = read only when mentioned, `ship/` = how it reaches the assessor. `entry.md` and `spec-to-plan.md` sit at the reference root — entry is the routing header, spec-to-plan the PDF entry point.
