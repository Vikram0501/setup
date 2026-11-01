# Prompt Templates (Qoder)

**Covers:** Copy-paste prompts + one-time Qoder setup that maximise output quality per token.
**Send when:** First doc to reach for — BUILD prompts pair with `reference/entry.md` + the spec PDF.
**Assumes:** Qoder IDE/Quest. Qoder rules limit: 100k chars total, natural language only.

---

## The five levers (why these prompts look like this)

1. **Input:** send one entry doc, not a doc pile — the agent routes itself and reads only what it needs (unread docs cost 0).
2. **Repetition:** never re-paste a doc. Refer to it by name ("per `tech-stacks.md`") — it's already in context.
3. **Output:** demand terseness explicitly. Verbosity is the biggest token sink — cut explanations, not requirements.
4. **Correctness:** "done" must mean *build verified*, not *code written*. Make verification a command, not a hope.
5. **No ping-pong:** forbid clarifying questions; require assumptions instead. Pauses are scheduled checkpoints (entry.md), not negotiations.

---

## Master prompt — BUILD (create an app)

```
You are a senior full-stack engineer. Deliver a working, demoable solution in stages with review checkpoints.

TASK
<paste the assessment prompt verbatim>

SPEC
<path to the spec PDF>

SETUP DOCS
<path to reference/entry.md> — read this first; follow its routing, checkpoint and commit rules exactly.

TIME (optional — delete if unknown)
<hours, e.g. 4 hours — pace checkpoints to this budget>

RULES
- Route to only the docs entry.md selects; never read the whole setup folder. Refer to docs by name, never re-paste.
- No clarifying questions — list assumptions instead. Work autonomously between checkpoints; stop only at the ones entry.md defines.
- TypeScript strict. No TODOs, stubs, or dead buttons — every stated feature must work.
- Terse output at every checkpoint (≤30 lines): status → proof (commands actually run) → new assumptions → next steps.
- Commit and push automatically per git-methodology.md at every green milestone — never ask.
- Dependencies limited to what the routed docs sanction.

CHECKPOINT 1 — PLAN: emit the plan in spec-to-plan.md format, then STOP and wait for confirmation.
CHECKPOINT 2+: report status + proof, then wait for "continue".
FINAL: definition-of-done.md run. DONE = clean clone → install → build passes → app runs → repo pushed.
```


## Master prompt — FIX (something is broken)

```
Fix the failing behaviour below. Do not refactor unrelated code.

FAILURE
<error output / wrong behaviour / reproduction steps>

RULES
- Diagnose first: state root cause in ≤3 lines before editing.
- Minimal diff. No dependency swaps, no architecture changes.
- Terse output. No explanations beyond root cause + what changed.
- Re-run the failing command to prove the fix before reporting.

OUTPUT: root cause (≤3 lines) → changed files → proof it passes.
```

## Master prompt — DEPLOY

```
Deploy this project and leave it runnable by others.

DOCS (attached): deployment-strategies.md

RULES
- Follow the selection table; don't evaluate alternatives.
- No secrets in repo. Bind 0.0.0.0, respect PORT env.
- If no hosting available, fall back to "repo quality only" and say so.
- Verify the deployed URL (or fresh-clone run) before reporting.

OUTPUT: strategy chosen (1 line) → URL/run proof → any env vars a reviewer must set.
```

## Master prompt — REVIEW / VERIFY

```
Verify this project against its prompt. Read-only: report issues, change nothing.

OUTPUT — one line per finding only:
[severity] file:line — problem → fix
End with: BLOCKERS (n) / MINOR (n) and a verdict: SHIP or FIX FIRST.
No praise, no summaries, no code rewrites unless a fix is a one-liner.
```

---

## One-time Qoder setup (do this once, before the assessment)

### 1. Rules — invariants that inject on every request

Create `.qoder/rules/always-apply.md` (type: **Always Apply**):

```markdown
- Output style: terse. Never explain code, restate files, or narrate plans beyond 3 lines.
- Never ask questions. Make reasonable assumptions and append them as a numbered list.
- TypeScript strict; no `any` without justification.
- Never declare done without running the build (and tests if present) — fix failures first.
- When the task defines checkpoints (see entry.md), stop there and wait — never skip ahead.
- Attached reference docs override framework defaults.
```

Only inviolables belong here — everything task-specific goes in the prompt.

### 2. Custom command — the build brief as a slash command

Create project command `.qoder/commands/build.md` with the **Master prompt — BUILD** body. Invoke:

```
/build <paste assessment prompt> + <PDF path> + <entry.md path>
```

Result: the boilerplate never gets re-typed, and you can't forget a rule. Add `fix`, `deploy`, `review` commands the same way. User-level path: `~/.qoder/commands/` (all projects); organize into subfolders if you add many.

### 3. Doc delivery — how docs reach the agent

| Method | When |
|---|---|
| **Setup repo in project** (`./setup/`) + one pointer to `reference/entry.md` | **Standard flow** — agent routes itself, reads only what the spec implies; unread docs cost 0 tokens |
| **Paste entry.md inline** | Setup repo can't be cloned — paste entry.md + the 2–3 docs its routing names |
| **AGENTS.md** in project root | Qoder auto-recognises it — use for the rules in step 1, not task docs |

FIX / DEPLOY / REVIEW prompts still attach their named docs directly (see each prompt) — no entry doc needed for those.

### 4. Quest vs Editor

- **Editor + Chat:** you're driving, tasks < ~10 min, want to steer.
- **Quest:** autonomous multi-step runs (full build/deploy) — hand off the prompt, track on the task board, review artifacts after.

### 5. Prompt enhancement

Qoder can rewrite your prompt before sending. **Skip it when you've attached docs** — it inflates input without adding information. It's useful only when your raw prompt is a bare one-liner.

---

## Token-saving checklist (before every send)

Levers 1–5 above, checked: only the entry doc attached (agent self-routes) · docs in context referenced by name, not re-pasted · no pleasantries or duplicated context · checkpoint output caps stated · rules file has invariants only · clarifying questions disabled.
