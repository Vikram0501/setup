# Testing

**Covers:** Minimum test suite that proves the app works without eating the clock.
**Send when:** Prompt asks for tests, or you need evidence for the REVIEW prompt / definition of done.
**Assumes:** Vitest; supertest for APIs; Playwright only if E2E is explicitly demanded.

---

## The hackathon pyramid

| Layer | What | How many | When |
|---|---|---|---|
| Unit | Pure logic (calculators, validators, mappers) | Every non-trivial function | As you write them |
| Integration | API routes against a real SQLite file | 5–10: happy path per endpoint + 1–2 failures | When endpoints exist |
| E2E (UI) | Playwright happy paths | 2–3 max | Only if the prompt demands it |

Skip: component snapshots, CSS tests, testing libraries' own behaviour, coverage targets.

## Setup (stack C; adapt for A)

```bash
npm i -D vitest supertest @types/supertest
```

```json
"scripts": { "test": "vitest run" }
```

- Each API test: temp DB file → migrate → seed → `request(app)` → assert status + body → delete file. Never point tests at dev data.
- Config: `vitest.config.ts`, `environment: "node"`, include `src/**/*.test.ts`.

## What to test (ranked)

1. Zod boundary: invalid body → 400 with `error.code === "VALIDATION_FAILED"`.
2. Auth: no token → 401; wrong role → 403; correct role → 200.
3. Each endpoint's happy path — one create → read → update → delete lifecycle, not every verb duplicated.
4. Business rules that would embarrass you in a demo (totals, permissions, state transitions).
5. One "not found" → 404.

## Example

```ts
import { describe, it, expect } from "vitest";
import request from "supertest";
import { app } from "../index";

describe("POST /api/tasks", () => {
  it("rejects missing title", async () => {
    const res = await request(app).post("/api/tasks").send({});
    expect(res.status).toBe(400);
    expect(res.body.error.code).toBe("VALIDATION_FAILED");
  });
});
```

## Definition of test-done

- [ ] `npm test` exits 0 from a clean clone
- [ ] At least one failing-path test per endpoint family
- [ ] Tests depend on no network, no dev server, no existing dev data
- [ ] CI (if present) runs `npm test`
