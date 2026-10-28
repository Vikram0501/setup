# API & Data Conventions

**Covers:** REST naming, response/error shapes, validation, pagination, dates and schema rules.
**Send when:** Building any backend portion (stacks A–C) — attach with `tech-stacks.md`.
**Assumes:** Hono/Express + Zod; Prisma or Drizzle over SQLite.

---

## Routes

- Prefix everything: `/api/...`
- Plural nouns: `/api/tasks`, `/api/tasks/:id`. No verbs in paths — `/api/tasks/complete` → `PATCH /api/tasks/:id` with `{ "completed": true }`.
- One nesting level max: `/api/projects/:projectId/tasks`.
- Collections: `?page=1&limit=20` (default 20, max 100).

## Response shapes

```jsonc
// single resource
{ "data": { "id": "t1", "title": "Write exam" } }

// collection
{ "data": [], "meta": { "page": 1, "limit": 20, "total": 57 } }
```

## Status codes

| Code | When |
|---|---|
| 200 / 201 | Success (201 + `Location` header on create) |
| 204 | Delete success, no body |
| 400 | Zod validation failed |
| 401 | Not authenticated |
| 403 | Authenticated but forbidden (role / ownership) |
| 404 | Missing — also for other users' resources; don't leak existence |
| 409 | Duplicate / conflict |
| 500 | Unexpected — log server-side, generic message client-side |

## Error shape (every non-2xx)

```json
{ "error": { "code": "VALIDATION_FAILED", "message": "title is required", "details": [] } }
```

Codes: `VALIDATION_FAILED`, `UNAUTHORIZED`, `FORBIDDEN`, `NOT_FOUND`, `CONFLICT`, `INTERNAL`.

## Validation & types

- Zod at **every** route boundary — body, query, params. Parse failure → 400 with issues.
- Export inferred types (`z.infer`); share between client and server (stack B: `/shared`). Never hand-write a second DTO type.

## Data rules

| Thing | Rule |
|---|---|
| IDs | Expose whatever the ORM gives, consistently (`id` on every resource) |
| Dates | ISO 8601 strings, stored UTC |
| Money | Integer cents + currency field. Never floats |
| Booleans | `completed`, `isActive` — never "yes"/"no" strings |
| Deletes | Hard delete unless the prompt asks for soft delete |
| Timestamps | Every model gets `createdAt`, `updatedAt` |

## Schema conventions (Prisma / Drizzle)

- Models PascalCase → plural tables (`Task` → `tasks`); fields camelCase.
- Required relations stated; `onDelete: Cascade` on owned children.
- One migration, committed. Deploy path runs `prisma migrate deploy`, never `migrate dev`.
