# Setup Docs

Repo of self-contained docs to attach to an AI coding agent (Qoder) during timed build assessments. Pick per task, paste, build — no time wasted deciding stack, colours, or deploy path.

## How to use

1. Read the prompt. Run `meta/kickoff-checklist.md` (classify + first 10 minutes) and `meta/time-budget.md` (pick your phase split).
2. Open `meta/prompt-templates.md`, copy the matching master prompt (BUILD / FIX / DEPLOY / REVIEW).
3. Attach 0–2 reference docs (3–4 for UI builds) — only what the task needs.
4. First time on a machine: do the one-time Qoder setup in `meta/prompt-templates.md` (rules + `/build` command).

## Repo layout

```
setup/
├── README.md
├── meta/                     # for you — never attach to the agent
│   ├── prompt-templates.md   # master prompts + Qoder setup
│   ├── kickoff-checklist.md  # first 10 minutes
│   ├── stack-selector.md     # → stack letter A–G
│   ├── deploy-selector.md    # → strategy 1–6
│   └── time-budget.md        # phase splits & kill criteria
└── reference/                # attach to the agent
    ├── spec-to-plan.md       # PDF/long spec → implementation plan
    ├── build/                # choosing & structuring the build
    │   ├── tech-stacks.md
    │   ├── project-scaffolds.md
    │   └── api-and-data-conventions.md
    ├── ui/                   # anything visual
    │   ├── colour-schemes.md
    │   ├── typography.md
    │   └── ui-layout.md
    ├── features/             # attach when the spec mentions them
    │   ├── auth.md
    │   └── testing.md
    └── ship/                 # deploy, containers, done-criteria
        ├── deployment-strategies.md
        ├── docker-ci.md
        └── definition-of-done.md
```

## Doc index

### Meta (for you — don't attach to the agent)

| Doc | Send when |
|---|---|
| `meta/prompt-templates.md` | Always — the prompt to send + Qoder setup |
| `meta/kickoff-checklist.md` | Task just released — first 10 minutes |
| `meta/stack-selector.md` | Stack not dictated; you're hesitating |
| `meta/deploy-selector.md` | Must host; need the strategy number fast |
| `meta/time-budget.md` | Task released — allocate the hours |

### Reference (attach to the agent)

| Doc | Send when |
|---|---|
| `reference/spec-to-plan.md` | Spec arrives as a PDF / long brief — attach with the PDF |
| `reference/build/tech-stacks.md` | Starting any build; stack not dictated |
| `reference/build/project-scaffolds.md` | Starting a build; want the exact skeleton |
| `reference/build/api-and-data-conventions.md` | Building any backend/API portion |
| `reference/ui/colour-schemes.md` | Task has a UI |
| `reference/ui/typography.md` | UI task — pairs with colour-schemes row-for-row |
| `reference/ui/ui-layout.md` | UI has multiple screens, navigation or a dashboard |
| `reference/features/auth.md` | Prompt mentions login, accounts or roles |
| `reference/features/testing.md` | Prompt asks for tests; or REVIEW needs proof |
| `reference/ship/deployment-strategies.md` | Must run somewhere other than your machine |
| `reference/ship/docker-ci.md` | Docker, containers, CI/pipeline asked |
| `reference/ship/definition-of-done.md` | Final pass before submit |

## Which docs for which prompt

| Prompt signals | Prompt | Attach |
|---|---|---|
| Spec arrives as PDF / long brief | BUILD | spec-to-plan + the PDF (+ 1–2 topical docs from rows below) |
| "Build a …" web app / CRUD / full-stack | BUILD | tech-stacks + colour-schemes + typography (+ ui-layout if multi-screen) |
| "Build a …" API / backend / CLI / script | BUILD | tech-stacks (+ api-and-data-conventions if endpoints) |
| Prompt also mentions login / accounts / roles | BUILD | + auth.md |
| Prompt also mentions tests / coverage | BUILD | + testing.md |
| Prompt also mentions Docker / CI / pipeline | BUILD | + docker-ci.md |
| "Fix", "not working", error output | FIX | (none) |
| "Deploy", "host", "live URL", "Docker" | DEPLOY | deployment-strategies (+ deploy-selector for you) |
| "Review", "verify", "check my solution" | REVIEW | tech-stacks (if stack compliance matters) + definition-of-done |
| Time-critical, design irrelevant | BUILD | tech-stacks only — system font stack + Tailwind default palette |

Rule of thumb: **max 2 reference docs per send** (UI builds may take 3–4: tech-stacks + colour-schemes + typography, + ui-layout only if multi-screen). Auth/testing/docker-ci are conditional extras. Every attached doc is billed in full, every request.

## Conventions

Every reference doc is self-contained (no cross-references required) with a `Covers / Send when / Assumes` header so it can be picked and sent in seconds. Docs refer to each other by bare filename only — folder location never matters when attaching. Meta docs are for you — never attach them.

Folder rule of thumb: `build/` = how the code is structured, `ui/` = how it looks, `features/` = attach only when mentioned, `ship/` = how it reaches the assessor. `spec-to-plan.md` sits at the reference root because it's the entry point for PDF specs.

## Status

- [x] Round 1: colour-schemes, tech-stacks, deployment-strategies
- [x] Prompt templates (Qoder)
- [x] ui-layout
- [x] typography
- [x] Round 2: kickoff-checklist, stack-selector, deploy-selector
- [x] Round 3: project-scaffolds, api-and-data-conventions, definition-of-done
- [x] Round 4: auth, testing, docker/CI, time-budget
- [x] spec-to-plan (PDF → implementation plan)
