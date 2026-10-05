# Prompt Templates (Qoder)

**Covers:** Copy-paste prompts + one-time Qoder setup that maximise output quality per token.
**Send when:** First doc to reach for — pair with 1–2 reference docs per task.
**Assumes:** Qoder IDE/Quest. Qoder rules limit: 100k chars total, natural language only.

---

## The five levers (why these prompts look like this)

1. **Input:** send only the docs the task needs — each attached doc is paid for once, in full.
2. **Repetition:** never re-paste a doc. Refer to it by name ("per `tech-stacks.md`") — it's already in context.
3. **Output:** demand terseness explicitly. Verbosity is the biggest token sink — cut explanations, not requirements.
4. **Correctness:** "done" must mean *build verified*, not *code written*. Make verification a command, not a hope.
5. **No ping-pong:** forbid questions; require assumptions instead. Every clarification round costs a full context reload.

---

## Master prompt — BUILD (create an app)

```
You are a senior full-stack engineer. Deliver a working, demoable solution.

TASK
<paste the assessment prompt verbatim>

DOCS (attached — apply silently, do not quote back)
- <doc 1, e.g. tech-stacks.md>
- <doc 2, e.g. colour-schemes.md>

RULES
- Choose per the attached docs. Decide fast; do not ask questions — list assumptions instead.
- TypeScript strict. No TODOs, stubs, or dead buttons — every stated feature must work.
- Terse output: no code explanations, no restating files, no summaries of what you're about to do.
- One pass: plan → implement → verify → report. Don't ask for permission between steps.
- Dependencies limited to what the attached docs sanction.

DONE = clean clone → install → build passes → app runs. Verify before reporting.

OUTPUT (keep under 30 lines)
1. Assumptions (≤5 bullets)
2. File tree
3. Built + verified: what works, how you proved it
4. Run commands / URL
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
- Attached reference docs override framework defaults.
```

Only inviolables belong here — everything task-specific goes in the prompt.

### 2. Custom command — the build brief as a slash command

Create project command `.qoder/commands/build.md` with the **Master prompt — BUILD** body. Invoke:

```
/build <paste assessment prompt> + attach chosen docs
```

Result: the boilerplate never gets re-typed, and you can't forget a rule. Add `fix`, `deploy`, `review` commands the same way. User-level path: `~/.qoder/commands/` (all projects); organize into subfolders if you add many.

### 3. Doc delivery — how to attach reference docs

| Method | When |
|---|---|
| **Paste inline** in first message | 1–2 docs, short tasks — fastest |
| **Repo folder** (`./setup-docs/`) + "read X before building" | Docs are large or agent may not need all of them — unread docs cost 0 tokens |
| **AGENTS.md** in project root | Qoder auto-recognises it — use for the rules in step 1, not task docs |

Never attach all docs "just in case" — every attached doc is billed in full, every request.

### 4. Quest vs Editor

- **Editor + Chat:** you're driving, tasks < ~10 min, want to steer.
- **Quest:** autonomous multi-step runs (full build/deploy) — hand off the prompt, track on the task board, review artifacts after.

### 5. Prompt enhancement

Qoder can rewrite your prompt before sending. **Skip it when you've attached docs** — it inflates input without adding information. It's useful only when your raw prompt is a bare one-liner.

---

## Token-saving checklist (before every send)

- [ ] Only docs this task needs are attached (0–2 usually)
- [ ] Docs already in context are referenced by name, not re-pasted
- [ ] Prompt has no pleasantries, no duplicated context, no long rationale
- [ ] Output limits stated (line caps force short answers)
- [ ] Rules file contains invariants only — nothing task-specific
- [ ] Question-asking disabled → one round instead of five
