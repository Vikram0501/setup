# Auth

**Covers:** Minimal real auth for stacks A and C — signup, login, session, roles — without OAuth/verification detours.
**Send when:** Prompt mentions accounts, login, signup, roles or protected routes.
**Assumes:** Stack A (Next.js + NextAuth) or C (Hono/Express + JWT). SQLite.

---

## Decide in 30 seconds

| Prompt needs | Do |
|---|---|
| "users can sign up / log in" | Real auth — this doc |
| "protected admin page" only | One hardcoded role check + gate page; skip full auth |
| Just "per-user data" in a demo | Skip auth entirely; state the assumption |

## Schema (both stacks)

```prisma
model User {
  id           String   @id @default(cuid())
  email        String   @unique
  passwordHash String
  name         String?
  role         String   @default("user") // "user" | "admin"
  createdAt    DateTime @default(now())
}
```

- Hash with bcrypt (12 rounds) or argon2. Never store or log plaintext.
- Min password 8 chars — enforce in Zod, not just the UI.

## Stack A — NextAuth (Auth.js) credentials

```bash
npm i next-auth bcryptjs
```

- Credentials provider: `authorize()` looks up the email, `bcrypt.compare`, return user or null.
- `auth: { strategy: "jwt" }` — no session table under time pressure.
- `NEXTAUTH_SECRET` in `.env` (`openssl rand -base64 32`); never commit.
- Session callback: stash `id` + `role` on the token so client and server can gate.
- Protect pages via middleware matcher `["/dashboard/:path*", "/admin/:path*"]`.
- API routes: `const session = await auth(); if (!session) return 401`.

## Stack C — JWT sessions (Hono / Express)

```bash
npm i bcryptjs jsonwebtoken
```

- `POST /api/auth/register` (Zod: email, password ≥ 8, name) → hash → create → 201.
- `POST /api/auth/login` → verify → sign `{ sub: id, role }` with `JWT_SECRET`, 1h expiry → return token.
- Middleware: `Authorization: Bearer <token>` → verify → attach `user` → else 401.
- Role gate: `requireRole("admin")` on admin routes → 403.
- Client stores token in an httpOnly cookie (preferred; note CORS) or memory.

## Authorisation rules (both)

- 401 = no/invalid session. 403 = logged in but not allowed. Don't return 403 for "doesn't exist" when ownership is unclear — return 404.
- Every mutating route checks ownership or role **server-side**. Hiding in the UI is not security.

## Explicitly skip under time pressure

Email verification, password reset, OAuth providers, refresh tokens, MFA, rate limiting, session tables. List them as future work in the README.
