# Stack Selector

**Covers:** 60-second decision tree that outputs one stack letter (A–G).
**Send when:** Stack isn't dictated and you're hesitating. Decide here, then attach `tech-stacks.md`.
**Assumes:** Nothing. Router for you — don't attach it to the agent.

---

## The tree (answer in order, stop at first yes)

1. Prompt demands a specific stack? → **use it. Tree over.** (Never override the prompt.)
2. Real-time bidirectional (chat, live collaboration)? → **F**
3. UI required?
   - One deployable app, no explicit client/server split → **A** (default)
   - Explicit SPA + separate API, or "decoupled" → **B**
   - Static only / "no backend" → **G**
4. Headless (API, webhook, service)? → **C**
5. Terminal (CLI, script, automation)? → **D**
6. Data transform / analysis (CSV, JSON, ETL)? → **E**
7. Still unsure? → **A.** It's the default for a reason.

## Lock-in rules

| Rule | Why |
|---|---|
| Decide by minute 5, never revisit | Mid-task stack switches lose the assessment |
| One database, one process | Microservices and queues fail demos |
| Prompt-named tools beat this tree | Golden rule 1 in `tech-stacks.md` |
| Lab machine on Node 18? A→Next 14, B→Vite 5 | Newer majors need Node 20+ |

## Signal cheat-sheet (if the tree felt ambiguous)

| Phrase in prompt | Stack |
|---|---|
| "web app", "full-stack", "dashboard" (all-in-one) | A |
| "SPA", "separate frontend and backend" | B |
| "REST API", "endpoint", "service" | C |
| "CLI", "script", "automation" | D |
| "process this CSV", "analyse the data" | E |
| "chat", "live updates" | F |
| "landing page", "portfolio" | G |
