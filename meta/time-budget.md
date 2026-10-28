# Time Budget

**Covers:** Phase allocation for 2h / 4h / 8h tasks, kill criteria, and what to sacrifice first.
**Send when:** Task released — pick your budget row before starting. Re-check at every checkpoint.
**Assumes:** `kickoff-checklist.md` completed.

---

## Phase split

| Phase | % of budget | 2h | 4h | 8h |
|---|---|---|---|---|
| Scaffold + green build | 10% | 12 min | 24 min | 48 min |
| Vertical slice (one entity end-to-end) | 30% | 36 min | 72 min | 2.4h |
| Breadth (remaining features) | 30% | 36 min | 72 min | 2.4h |
| UI polish + states | 15% | 18 min | 36 min | 1.2h |
| Deploy + README + DoD pass | 15% | 18 min | 36 min | 1.2h |

Tests are woven into each phase (`testing.md`), not saved for the end — unless the prompt weights tests heavily, then take the time from "breadth".

## Checkpoints

| Milestone | Deadline | If late |
|---|---|---|
| Green scaffold committed | 10% mark | Cut lint/format config; accept defaults |
| Vertical slice demoable | 40% mark | Cut secondary features NOW — list them in README as not built |
| Feature freeze | 85% mark | No new features after this; only fixes, polish, deploy |
| Definition-of-done run | 95% mark | Anything failing here was never done — fix or declare a gap |

## Kill criteria (stop the bleeding immediately)

- Same bug for **20 minutes** with no new insight → stub or feature-flag it, move on, return at freeze if time remains.
- Dependency install / build broken > 10 min → swap to the fallback stack in `tech-stacks.md`; don't debug tooling.
- Deploy blocked > 15 min → strategy 5 (repo quality) + README note. A perfect local app beats a 500-ing URL.

## Sacrifice order (first listed dies first)

1. E2E / UI tests → keep unit + API tests
2. Dark mode / second palette variant
3. Animations, icons, non-essential components
4. The prompt's "bonus" / nice-to-have features
5. Live deploy → repo-quality fallback — **unless** the prompt demands a URL as a hard requirement; then invert and sacrifice polish instead

## The demoable floor

If everything else dies, ship: clean clone → install → build → one working core flow + a README stating exactly what works. That passes more assessments than a half-broken feature set.
