# Definition of Done

**Covers:** The verifiable checklist that turns "code written" into "done" — run it before every submit.
**Send when:** Any build task; attach with the BUILD prompt, or self-check at the end.
**Assumes:** JS/TS stacks from `tech-stacks.md`.

---

## Build & run (all must pass, in this order)

```bash
git clone <repo> && cd <app>
npm ci
npm run lint
npm run build
npm test            # if tests exist
npm run dev         # or npm start
```

- [ ] Every command above exits 0 on a clean clone
- [ ] App serves on the port it prints; no crash loops
- [ ] Zero errors in the browser console on main flows

## Features

- [ ] Every prompt requirement works end-to-end
- [ ] No dead buttons, TODO comments, or placeholder text in the UI
- [ ] Data survives refresh (DB/file, not component state) unless the prompt allows in-memory
- [ ] Empty, loading and error states for each data view
- [ ] Form input validated; bad input shows a message, never a silent fail

## UI (if applicable)

- [ ] Works at 375px and 1280px; no horizontal scroll
- [ ] One primary action per screen; active nav state visible
- [ ] Same header / button placement across pages

## Repo hygiene

- [ ] README: setup, run, build, test commands in ≤ 10 lines
- [ ] `.env.example` committed; `.env` git-ignored; no secrets in history
- [ ] No `node_modules`, `dist`, or `*.db` committed
- [ ] `npm run seed` creates demo data (if applicable)

## Deploy (only if asked)

- [ ] Live URL loads; one real data path exercised post-deploy
- [ ] Host env vars documented for the reviewer

## Verdict

Anything unchecked → not done. If time ran out: leave it unchecked and add a **Known gaps** section to the README — never silently ship a broken path.
