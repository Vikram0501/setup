# Setup Docs

Repo of self-contained docs to attach to an AI coding agent (Qoder) during timed build assessments. Pick per task, paste, build — no time wasted deciding stack, colours, or deploy path.

## How to use

1. Read the prompt and classify the task with the table below.
2. Open `meta/prompt-templates.md`, copy the matching master prompt (BUILD / FIX / DEPLOY / REVIEW).
3. Attach 0–2 reference docs — only what the task needs.
4. First time on a machine: do the one-time Qoder setup in `meta/prompt-templates.md` (rules + `/build` command).

## Doc index

| Doc | Send when |
|---|---|
| `meta/prompt-templates.md` | Always — the prompt to send + Qoder setup |
| `reference/colour-schemes.md` | Task has a UI |
| `reference/tech-stacks.md` | Starting any build; stack not dictated |
| `reference/deployment-strategies.md` | Must run somewhere other than your machine |

## Which docs for which prompt

| Prompt signals | Prompt | Attach |
|---|---|---|
| "Build a …" web app / CRUD / full-stack | BUILD | tech-stacks + colour-schemes |
| "Build a …" API / backend / CLI / script | BUILD | tech-stacks |
| "Fix", "not working", error output | FIX | (none) |
| "Deploy", "host", "live URL", "Docker" | DEPLOY | deployment-strategies |
| "Review", "verify", "check my solution" | REVIEW | tech-stacks (if stack compliance matters) |
| Time-critical, design irrelevant | BUILD | tech-stacks only — use Tailwind default palette |

Rule of thumb: **max 2 reference docs per send.** Every attached doc is billed in full, every request.

## Conventions

Every doc is self-contained (no cross-references required) with a `Covers / Send when / Assumes` header so it can be picked and sent in seconds.

## Status

- [x] Round 1: colour-schemes, tech-stacks, deployment-strategies
- [x] Prompt templates (Qoder)
- [ ] Round 2: kickoff-checklist, stack-selector, deploy-selector
- [ ] Round 3: project-scaffolds, api-and-data-conventions, definition-of-done
- [ ] Round 4: auth, testing, docker/CI, typography, time-budget
