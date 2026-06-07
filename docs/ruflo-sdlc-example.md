# Welcome to Ruflo SDLC 👋

End-to-end software development with Ruflo — from research to production.

## Common Tasks

| Category | What I Can Do |
|----------|--------------|
| **Research** | Deep research, synthesize findings, explore codebase |
| **Architecture** | Write ADRs, domain design, bounded contexts |
| **Implementation** | Write code, refactor, coordinate multi-agent swarms |
| **Git** | Branch, commit, create PRs, manage issues |
| **Debug** | Trace bugs, fix errors, run tests |
| **Review** | Code review, security review, merge to main |

## Quick Commands

| Command | Purpose |
|---------|---------|
| `/ruflo-adr:adr-create` | Create an Architecture Decision Record |
| `/ruflo-adr:adr-review` | Review code against accepted ADRs |
| `/ruflo-adr:adr-index` | Rebuild ADR index and dependency graph |
| `/ruflo-adr:adr-verify` | Check for dangling refs or supersede cycles |
| `/ruflo-goals:deep-research` | Multi-phase deep research with web search |
| `/ruflo-goals:goal-plan` | GOAP action plan with cost optimization |
| `/code-review` | Review the current diff |
| `/review` | Review a pull request |
| `/security-review` | Security review of pending changes |
| `/init` | Create a CLAUDE.md for a project |

## How to Ask

Just describe what you need naturally. Here are prompts for every phase of the development lifecycle, following one feature from idea to production.

### Research & Discovery

| Prompt | What Ruflo Uses |
|--------|----------------|
| *"Research OAuth2 best practices for a web app with a separate API backend"* | Deep research skill — web search, codebase scan, synthesis |
| *"What auth middleware already exists in this codebase?"* | Grep + Glob to find existing patterns |

### Synthesize

| Prompt | What Ruflo Uses |
|--------|----------------|
| *"Summarize the OAuth2 research and recommend an approach with trade-offs"* | Synthesizes research findings into a decision brief |
| *"Compare PKCE vs implicit flow for our architecture"* | Embeddings search + web search for comparison |

### Architecture Decision Record

| Prompt | What Ruflo Uses |
|--------|----------------|
| *"Write an ADR for using GitHub OAuth2 with PKCE for authentication"* | `/ruflo-adr:adr-create` — creates numbered ADR, registers in index |
| *"Review this code against ADR-015"* | `/ruflo-adr:adr-review` — checks code for ADR compliance |

### Domain Design (DDD)

| Prompt | What Ruflo Uses |
|--------|----------------|
| *"Design the domain model for OAuth2 login with User, Session, and AuthMiddleware bounded contexts"* | System-architect agent — TypeScript interfaces, aggregates, value objects |
| *"Swarm: design Identity, Session, and API Gateway domains in parallel, then review"* | Hierarchical swarm — per-domain architects + reviewer agent |

### Implementation

| Prompt | What Ruflo Uses |
|--------|----------------|
| *"Add OAuth2 login with GitHub provider to the API"* | Coder agent — reads codebase patterns, writes matching code |
| *"Swarm: implement the OAuth2 feature — architect designs, coders implement, reviewer checks"* | Hierarchical swarm — architect → coder → reviewer pipeline |

### Git Workflow

| Prompt | What Ruflo Uses |
|--------|----------------|
| *"Create a feature branch, commit the OAuth2 changes, and open a PR"* | Git + GitHub MCP tools — branch, commit, push, create PR |
| *"Squash-merge the OAuth2 PR into main"* | GitHub MCP — merge with squash |

### Bug Discovery & Tracing

| Prompt | What Ruflo Uses |
|--------|----------------|
| *"The OAuth2 callback is returning 500 — trace the error through passport config and session serialization"* | Debugger agent — follows stack trace, reads files, identifies root cause |
| *"Find why the login button isn't redirecting after authentication"* | Grep + Read — traces client-side redirect logic |

### Bug Fixes

| Prompt | What Ruflo Uses |
|--------|----------------|
| *"Fix the serializeUser bug — it should use user._id instead of user.id"* | Edit tool — targeted 1–2 file fix |
| *"Fix the CORS error on the OAuth2 callback route"* | Edit tool — adds missing headers to route handler |

### Testing

| Prompt | What Rufoo Uses |
|--------|-----------------|
| *"Write tests for the OAuth2 auth flow — unit tests for passport, integration test for the callback, component test for the login button"* | Tester agent — writes Vitest tests with mocks |
| *"Swarm: write unit, integration, and component tests in parallel, then review for coverage gaps"* | Hierarchical swarm — 3 test writers + 1 coverage reviewer |

### Final Review & Merge

| Prompt | What Ruflo Uses |
|--------|----------------|
| *"Review this PR for security issues"* | `/security-review` — checks for hardcoded secrets, CSRF, token leaks |
| *"Review the OAuth2 feature PR — check it matches ADR-015, types are consistent, and all error paths are handled"* | `/review` — reads full diff, checks architecture compliance |
