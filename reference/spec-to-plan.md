# Spec-to-Plan (PDF)

**Covers:** Extracting a working implementation plan from an attached PDF spec in one pass — no clarifying questions, no re-reading, minimal tokens.
**Send when:** The assessment arrives as a PDF or long-form brief. Attach with the PDF + 1–2 topical reference docs.
**Assumes:** BUILD master prompt in use. Reference docs already in context apply silently.

---

## The contract

- One pass: extract → classify → plan → implement. The plan is **not** a checkpoint — never wait for approval after emitting it.
- Never quote the PDF back. Work from the numbered requirement list you extract once (R1, R2, …) and refer to IDs thereafter.
- The only permitted question: "PDF unreadable — paste the text." Everything else becomes a numbered assumption.
- If a grading rubric exists in the PDF, **it is the spec**. Every rubric line becomes a requirement, in rubric order.

## Step 1 — Extract (once)

Pull these out of the PDF. Ignore intro prose, company/lecturer boilerplate, repeated content, and anything restating the template.

| Extract | Into |
|---|---|
| Every functional requirement | R1, R2, … (one line each, verbatim intent) |
| Data entities + fields mentioned | Entity list |
| Stack / tool / language mandates | Constraint list (hard) |
| Deliverables (repo, URL, video, report) | Deliverable list |
| Auth, deploy, testing mentions | Constraint flags (drives doc choice) |
| "Nice to have" / bonus wording | Stretch list (build only after MVP works) |

## Step 2 — Classify (signals → rules)

Apply in this order; the first match wins per area. If the matching reference doc isn't attached, follow the fallback column — don't attach more docs mid-task.

| Signal in spec | Apply | Fallback if doc absent |
|---|---|---|
| Stack named | Use it exactly | — |
| No stack, UI implied | `tech-stacks.md` stack A | Next.js + TS + Tailwind + SQLite |
| No stack, API/CLI implied | `tech-stacks.md` stack C / D | Hono + Zod + SQLite |
| UI + no stated design | `colour-schemes.md` row 1 + `typography.md` row 1 (Slate + Inter) | Neutral palette, 16px body |
| Dashboard / multi-screen | `ui-layout.md` archetype "App shell + sidebar" | Sidebar + one content column |
| Login / roles mentioned | `auth.md` | bcrypt + JWT, skip OAuth |
| Tests mentioned | `testing.md` pyramid | 5 API tests minimum |
| Live URL / host mentioned | `deployment-strategies.md` | Vercel (app) / Render (API) |
| Docker / CI mentioned | `docker-ci.md` | Multi-stage Dockerfile + GH Actions |
| Entity/endpoint work | `api-and-data-conventions.md` | `/api/plural`, `{data}` / `{error}` shapes |

## Step 3 — Plan output (fixed format, ≤ 40 lines)

```
ASSUMPTIONS (≤5, numbered)
STACK: letter + palette row + typography row (1 line)
REQUIREMENTS: R1…Rn mapped to features (1 line each)
MVP (build order): 1…5 — the vertical slice first, then breadth
STRETCH: list, or "none"
DATA: entities + one key field each
FILES: sketch of top-level tree only
VERIFY: the exact commands that prove done
DEPLOY: strategy number + fallback
```

Rules: MVP = the smallest set where every rubric/functional line is demoable. Stretch never delays MVP. Build order always starts with one entity end-to-end (UI → API → DB).

## Step 4 — Implement immediately

After emitting the plan: build, don't narrate. Per requirement, tick silently (R-status in your head, not in chat). Only re-mention a requirement if it failed verification or was cut for time.

## Ambiguity rules

| Conflict | Resolution |
|---|---|
| Explicit vs implied requirement | Explicit wins |
| Two features can't both fit time | Higher rubric weight first; else MVP list order |
| Unspecified design / branding | Neutral defaults (Step 2 fallbacks) |
| Data model vague | Smallest model that satisfies every R; add fields only when a requirement demands one |
| "Professional / production-ready" with no definition | `definition-of-done.md` checklist — nothing more |

## Anti-patterns

- Re-pasting or re-reading the PDF after extraction — use R-IDs.
- Emitting the plan and stopping for feedback — execute.
- Building stretch or polish before the MVP slice runs.
- Asking "which features matter?" — the rubric, else the R-order, else your assumptions.
- Gold-plating entities "for extensibility" — smallest schema that passes all R's.
