# Ruflo End-to-End SDLC Example

> A complete software development lifecycle walkthrough using Ruflo, following one feature from idea to production.
> Each phase shows the **exact prompt you'd type** — copy, paste, adapt.

## The Sample Feature

**Add OAuth2 login with GitHub provider** to an existing Next.js + Express API application.

This feature is concrete enough to exercise every SDLC phase: research, architecture decisions, domain design, implementation, debugging, testing, and release.

---

## Phase 1: Research & Discovery

> **Swarm?** No. This is exploration — you need to understand the landscape before delegating work.

### What & Why

Before writing any code, understand what OAuth2 patterns exist, what your codebase already has, and what constraints apply. A single agent reading and synthesizing is more efficient than a swarm here — you need one coherent picture, not parallel fragments.

### Type This

```
I need to add GitHub OAuth2 login to our Next.js + Express app. Research:
1. What OAuth2 flow is best for a web app with a separate API backend?
2. What does our codebase already have for authentication? Search for existing auth middleware, session handling, user models.
3. What are the security considerations (CSRF, token storage, PKCE)?
4. What npm packages are commonly used (next-auth, passport, etc.)?

Synthesize your findings into a summary with recommendations.
```

### What You'll Get

A cohesive research summary covering: the Authorization Code flow with PKCE, existing auth middleware in your codebase, security pitfalls, and a package recommendation.

---

## Phase 2: Synthesize & Decide

> **Swarm?** No. This is you thinking through the research output.

### What & Why

After research, you need to make key decisions before committing to architecture. These decisions get recorded in an ADR.

### Type This

```
Based on the research, I want to go with:
- Authorization Code flow with PKCE
- Passports GitHubStrategy on the Express side
- NextAuth.js for the Next.js client-side session management
- Store refresh tokens encrypted in the database, access tokens in memory only

Summarize this as a decision brief I can use to write an ADR. Include: context, decision, consequences (both positive and negative).
```

---

## Phase 3: Write the ADR

> **Swarm?** No. A single architectural decision record — one agent, one document.

### What & Why

Record the OAuth2 decision formally. ADRs are short-lived artifacts — a swarm would add coordination overhead for a task that takes one prompt.

### Type This

```
/ruflo-adr:adr-create

Title: ADR-015 — Use GitHub OAuth2 with PKCE for Authentication
Status: Proposed
Context: Our app currently has no user authentication. We need GitHub OAuth2 for developer-facing users.
Decision: Implement Authorization Code flow with PKCE using Passport.js (server) and NextAuth.js (client). Store refresh tokens encrypted in DB, access tokens in server memory only.
Consequences:
  - (+) Standards-based, well-understood flow
  - (+) PKCE prevents authorization code interception
  - (-) Two session systems (Passport + NextAuth) to keep in sync
  - (-) Refresh token encryption adds key management burden
```

### What You'll Get

An ADR file at `docs/adrs/ADR-015-oauth2-github-pkce.md`, registered in the ADR index.

---

## Phase 4: Domain Design (DDD)

> **Swarm?** Yes. DDD design touches multiple bounded contexts — parallel agents can design different contexts simultaneously, then merge.

### What & Why

OAuth2 login spans multiple domains: **Identity** (user entity, OAuth profiles), **Session** (tokens, refresh cycles), and **API Gateway** (middleware, route guards). Each domain can be designed concurrently by specialists.

### Type This

```
I need to design the domain model for GitHub OAuth2 login. Create a team to design three bounded contexts in parallel:

1. **Identity Domain**: User aggregate, GitHubProfile value object, OAuthCredentials value object. Define the repository interface.

2. **Session Domain**: AuthSession aggregate, AccessToken and RefreshToken value objects. Define token refresh policy and encryption requirements.

3. **API Gateway Domain**: AuthMiddleware service, route guard specifications, error handling for expired/invalid tokens.

Each architect: design your domain, write the TypeScript interfaces to .claude/skills/sdlc-oauth2-design/, then send your design to the next agent for review.
```

**Manual alternative (no swarm):**

```
Design the domain model for GitHub OAuth2 login. Cover three bounded contexts:
1. Identity Domain (User, GitHubProfile, OAuthCredentials)
2. Session Domain (AuthSession, AccessToken, RefreshToken)
3. API Gateway Domain (AuthMiddleware, route guards)

Write TypeScript interfaces for each. Show aggregate roots, value objects, and repository interfaces.
```

### When to Use Swarm vs. Solo

| Factor | Swarm | Solo |
|--------|-------|------|
| 3+ bounded contexts with clear boundaries | ✅ | |
| Tight coupling between domains (can't design independently) | | ✅ |
| Need multiple perspectives (security + domain + API) | ✅ | |
| Simple single-domain feature | | ✅ |

---

## Phase 5: Implementation

> **Swarm?** Yes. Implementation involves 5+ files across 3 layers (client, server, shared). Pipeline coordination prevents drift.

### What & Why

The OAuth2 feature touches the Express server (Passport setup, auth routes), the Next.js client (NextAuth config, login UI), the database (user schema, token storage), and shared types. A hierarchical swarm keeps the architect's design intact while coders implement in parallel.

### Type This

```
Implement the GitHub OAuth2 login feature based on the domain design from the previous phase.

Key files to create/modify:
- Server: src/server/auth/passport-config.ts, src/server/auth/routes.ts, src/server/auth/middleware.ts
- Client: src/client/lib/auth/nextauth-config.ts, src/client/components/login-button.tsx
- Shared: src/shared/types/auth.ts, src/shared/schemas/oauth.ts
- Database: migrations/001_add_oauth_fields.sql

Use hierarchical topology. The architect should review the design docs from Phase 4, then assign implementation to coders. Use SendMessage to hand off between phases.
```

**Manual alternative (no swarm, single complex prompt):**

```
Implement GitHub OAuth2 login based on the domain design. Create these files:
1. src/server/auth/passport-config.ts — Passport GitHubStrategy with PKCE
2. src/server/auth/routes.ts — /auth/github, /auth/github/callback, /auth/logout
3. src/server/auth/middleware.ts — requireAuth, optionalAuth guards
4. src/client/lib/auth/nextauth-config.ts — NextAuth GitHub provider
5. src/client/components/login-button.tsx — "Sign in with GitHub" button
6. src/shared/types/auth.ts — User, GitHubProfile, AuthSession interfaces
7. src/shared/schemas/oauth.ts — Zod validation schemas
8. migrations/001_add_oauth_fields.sql — Add oauth_provider, oauth_id, refresh_token columns

Use pnpm, Vite, Vitest, Next.js conventions. Follow existing patterns in the codebase.
```

### When to Use Swarm vs. Solo

| Factor | Swarm | Solo |
|--------|-------|------|
| 5+ files across multiple layers | ✅ | |
| 1–2 files, clear implementation path | | ✅ |
| Need architect oversight to prevent drift | ✅ | |
| Simple CRUD endpoint with 1 file | | ✅ |
| Security-sensitive code needs reviewer | ✅ | |

---

## Phase 6: Git Workflow

> **Swarm?** No. Git operations are sequential and require human judgment at decision points.

### What & Why

Branch, commit, and create a PR. This is a linear workflow — nobody benefits from parallel commits to the same branch.

### Type This

```
Create a feature branch for the OAuth2 work, stage all the auth files, and commit with message:

feat(auth): add GitHub OAuth2 login with PKCE

- Passport GitHubStrategy with PKCE on Express server
- NextAuth GitHub provider on Next.js client
- Encrypted refresh token storage
- Auth middleware with requireAuth and optionalAuth guards
- Login button component
- Migration for oauth fields on users table

Then push and create a PR against main.
```

### What You'll Get

A feature branch `feat/oauth2-github`, a well-structured commit, and a pull request ready for review.

---

## Phase 7: Bug Discovery & Tracing

> **Swarm?** No. Bug tracing is a single-threaded investigation. Swarms add noise.

### What & Why

You deployed to staging, clicked "Sign in with GitHub," and got a 500 error. Debugging is detective work — one agent following the trail is faster than a swarm that would trip over each other reading the same logs.

### Type This

```
The GitHub OAuth2 login is failing in staging. When I click "Sign in with GitHub", I get a 500 error after the GitHub callback redirects back. The server logs show:

  Error: Failed to serialize user into session
  at SessionManager.serializeUser (passport/lib/sessionmanager.js:42:19)

Trace this bug:
1. Check src/server/auth/passport-config.ts — is serializeUser defined?
2. Check the User type in src/shared/types/auth.ts — does the session serializer expect a field that doesn't exist on the OAuth user?
3. Check if the callback route in src/server/auth/routes.ts is handling the case where the user is new (first GitHub login) vs. returning.

Find the root cause and explain it.
```

### What You'll Get

Root cause analysis: the `serializeUser` callback expects a `user.id` field, but the GitHub OAuth flow creates the user with `_id` (MongoDB convention). The serializer and the user creation are out of sync.

---

## Phase 8: Bug Fix

> **Swarm?** No. This is a 1–2 file fix. A swarm would cost more in coordination overhead than the fix itself.

### What & Why

The bug is localized to the session serializer. A targeted fix is cleaner and faster than spinning up a team.

### Type This

```
Fix the session serialization bug. The issue is that serializeUser expects user.id but our User model uses user._id (MongoDB).

Update src/server/auth/passport-config.ts:
- Change serializeUser to use user._id.toString()
- Change deserializeUser to handle the string _id properly

Also update src/shared/types/auth.ts:
- Add an id getter or ensure the User interface documents the _id field

Keep the fix minimal — don't refactor the entire auth module for this.
```

### Commit the fix

```
Commit this bug fix with:

fix(auth): use _id for passport session serialization

Passport's serializeUser was referencing user.id which doesn't exist
on our MongoDB User model. Changed to user._id.toString() to match
the actual document structure.

Fixes: GitHub OAuth2 callback returning 500 after redirect
```

---

## Phase 9: Testing

> **Swarm?** Yes. Test suites for different layers can be written in parallel, and the reviewer catches gaps.

### What & Why

The OAuth2 feature needs unit tests (Passport strategy, middleware), integration tests (auth flow end-to-end), and component tests (login button). These layers are independent enough to write concurrently, and a reviewer agent catches cross-cutting gaps like missing edge-case coverage.

### Type This

```
Write comprehensive tests for the GitHub OAuth2 feature. Use a swarm with:
- 1 coder: unit tests for passport-config.ts, middleware.ts, and Zod schemas
- 1 coder: integration tests for the full auth flow (/auth/github → callback → session)
- 1 tester: component tests for the login button + NextAuth mock setup
- 1 reviewer: after all tests are written, review for missing edge cases (expired tokens, revoked access, rate limits, new user vs. returning user)

Use Vitest. Mock external GitHub API calls. Use SendMessage to send each test suite to the reviewer when done.
```

**Manual alternative (no swarm, for simple features):**

```
Write tests for the GitHub OAuth2 auth feature using Vitest:
1. Unit tests for passport-config.ts (GitHubStrategy config, serialize/deserialize)
2. Unit tests for middleware.ts (requireAuth blocks unauthenticated, optionalAuth passes through)
3. Unit tests for oauth schema validation (valid/invalid OAuth payloads)
4. Integration test for the login flow (mock GitHub API, test callback → session → redirect)
5. Component test for LoginButton (renders correctly, calls signIn on click)

Mock all external calls. Test both happy path and error cases (token expired, user revoked access, GitHub API down).
```

### When to Use Swarm vs. Solo

| Factor | Swarm | Solo |
|--------|-------|------|
| 3+ test layers (unit + integration + component) | ✅ | |
| 1–2 simple unit tests | | ✅ |
| Need reviewer to catch coverage gaps | ✅ | |
| All tests in one file | | ✅ |

---

## Phase 10: Final Review & Merge

> **Swarm?** No. Review and merge are judgment calls — one agent reading the full diff is more reliable than fragmented reviews.

### What & Why

A single reviewer seeing the complete picture catches cross-cutting issues (like a type mismatch between server and client) that split reviewers might miss.

### Type This

```
Review the OAuth2 feature PR before merge. Check:
1. Does the code match the ADR-015 decision (PKCE, encrypted refresh tokens, two session systems)?
2. Are there any security issues — hardcoded secrets, missing CSRF protection, token leaks in logs?
3. Do the TypeScript types match between src/shared/types/auth.ts and what the server/client actually use?
4. Are all error paths handled (GitHub API down, token refresh failure, user denied consent)?
5. Is the migration reversible?

Run the full test suite first. If everything passes, merge with squash.
```

---

## Quick Reference: Swarm vs. Solo Decision

| SDLC Phase | Typical Approach | When to Swarm | When to Go Solo |
|------------|-----------------|---------------|-----------------|
| Research | Solo | Multiple independent research targets | Single topic, one codebase |
| Synthesize | Solo | — | Always |
| ADR | Solo | — | Always |
| Domain Design | Depends | 3+ bounded contexts, need multi-perspective review | Single domain |
| Implementation | Depends | 5+ files, security-sensitive, need architect oversight | 1–2 files, clear path |
| Git Workflow | Solo | — | Always |
| Bug Tracing | Solo | — | Always |
| Bug Fix | Solo | — | 1–2 files (almost always) |
| Testing | Depends | 3+ test layers, need coverage reviewer | 1–2 simple tests |
| Review & Merge | Solo | — | Always |

### Swarm Anti-Patterns to Avoid

| ❌ Don't | ✅ Do |
|----------|-------|
| Swarm a 2-file bug fix | Fix it yourself — coordination overhead > work |
| Swarm research on one topic | One thorough investigation beats three shallow ones |
| Swarm a git commit | Sequential operations can't be parallelized |
| Swarm a single domain design | One coherent model > fragmented models stitched together |

---

## Complete Prompt Sequence (Copy-Paste Reference)

```bash
# Phase 1: Research
"I need to add GitHub OAuth2 login. Research OAuth2 flows, our existing auth code, security considerations, and npm packages. Synthesize findings."

# Phase 2: Synthesize
"Based on the research, I want Authorization Code + PKCE, Passport on server, NextAuth on client, encrypted refresh tokens in DB. Write a decision brief for an ADR."

# Phase 3: ADR
/ruflo-adr:adr-create  # (fill in the form with the decision brief)

# Phase 4: Domain Design (SWARM)
"Design the domain model for GitHub OAuth2 across 3 bounded contexts: Identity, Session, API Gateway. Spawn architects for each."

# Phase 5: Implementation (SWARM)
"Implement GitHub OAuth2 login based on the domain design. Create passport config, auth routes, middleware, NextAuth config, login button, shared types, schemas, and migration."

# Phase 6: Git
"Create feat/oauth2-github branch, commit all auth files, push, create PR against main."

# Phase 7: Bug Trace
"OAuth2 callback is failing with 'Failed to serialize user into session'. Trace through passport-config.ts, User type, and callback route to find root cause."

# Phase 8: Bug Fix
"Fix serializeUser to use user._id.toString() instead of user.id. Minimal change in passport-config.ts and auth.ts types."

# Phase 9: Testing (SWARM)
"Write tests for OAuth2: unit tests (passport, middleware, schemas), integration tests (full auth flow), component tests (login button), then review for coverage gaps."

# Phase 10: Review & Merge
"Review the OAuth2 PR: match ADR-015, check security, type consistency, error handling, migration reversibility. Run tests then squash-merge."
```

---

## Lifecycle Flow Diagram

```
Research ──→ Synthesize ──→ ADR ──→ Domain Design ──→ Implementation ──→ Git
  (solo)       (solo)     (solo)     (swarm)           (swarm)        (solo)
                                                                         │
                                                                         ▼
                            Merge ← Review ← Testing ← Bug Fix ← Bug Trace
                            (solo)   (solo)    (swarm)   (solo)     (solo)
```

*Boxes show swarm/solo recommendation. The feature flows left-to-right, then wraps back when bugs are found during testing.*
