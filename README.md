# Test Copilot

AI-powered test engineering agent for creating, reviewing, and improving test strategies and test cases for any type of software project.

It follows an **AI-DLC** workflow — **Generate → Human Validate → Execute** — split into two approval-gated steps so AI drafts and humans approve before any code is written.

## Installation

### Via Claude Code Marketplace (Recommended)

In Claude Code, type:

```
GitHub: Piyabordee/test-copilot
```

The plugin will be automatically discovered and installed.

### Manual Installation

See [INSTALLATION.md](INSTALLATION.md) for step-by-step manual setup.

## How to Use

**Step 1 — Generate a test strategy (read-only):**

```
/test-copilot:test-engineer
```

The agent analyzes the codebase, produces a complete test strategy, then **stops and asks you to review**. Iterate until you are satisfied, then reply `APPROVE`.

**Step 2 — Turn the approved strategy into code (writes files):**

```
/test-copilot:generate-tests
```

The agent generates runnable tests that mirror the approved cases, using your existing test framework, and conditionally proposes a CI pipeline.

## Command Overview

### /test-copilot:test-engineer — Strategy (Step 1 of 2)

Analyze the codebase and produce a test strategy and test cases for human review. Does **not** write files.

**Execution flow:**

1. Gathers business context, detects project type & test framework, and inventories existing tests
2. Invokes the `test-engineer-skill` for deep testing knowledge
3. Produces a complete test strategy and self-checks it
4. **Stops and asks for approval** — reply `APPROVE` when satisfied

### /test-copilot:generate-tests — Code (Step 2 of 2)

Turn the approved strategy into executable test code and CI config. **Writes files.** Requires an approved strategy in the conversation.

**Specializations:**

- REST API Testing (contract testing, rate limiting, security headers)
- GraphQL Testing (query complexity, nested resolvers, schema integrity)
- Mobile Application Testing (network conditions, device fragmentation, lifecycle)
- Web Application Testing (cross-browser, responsive design, accessibility)
- Microservices Testing (circuit breakers, service discovery, distributed tracing)
- Security Testing (OWASP Top 10 injection, auth, authorization, data exposure)
- Performance Testing (response time percentiles, throughput, load testing)

## Architecture

```
Step 1 — Strategy (read-only)          Step 2 — Code (writes files)
/test-copilot:test-engineer            /test-copilot:generate-tests
   │                                       │
   └── test-engineer-agent                 └── test-codegen-agent
        ├── Reconnaissance & Context             ├── Load approved strategy
        ├── Invoke Skill                         ├── Invoke Skill (codegen)
        ├── Generate Strategy                    ├── Generate code (existing framework)
        ├── Self-Check                           ├── Place files (ai-generated/)
        └── STOP → reply APPROVE ───────────▶    ├── CI & secrets (conditional)
                                                 └── Self-Check
                         │                               │
                         └────── test-engineer-skill ────┘
                          (knowledge, templates, checklists, codegen rules)
```

## Example

**Step 1** — in a Node.js REST API project, `/test-copilot:test-engineer` produces:

```
# Test Strategy: My API Project

## Detected Project Type
- Primary: REST API (Express.js)
- Test Framework: Jest + Supertest

## Test Cases

### Suite: Auth API (`/api/auth`)
| # | Test Case | Method | Endpoint | Input | Expected | Status |
|---|-----------|--------|----------|-------|----------|--------|
| 1 | Login with valid credentials | POST | /api/auth/login | {email, password} | 200 + JWT | Critical |
| 2 | Login with wrong password | POST | /api/auth/login | {email, "wrong"} | 401 + error | Critical |
| 3 | Rate limit after 5 failures | POST | /api/auth/login | 6x failed login | 429 + Retry-After | Critical |

### Recommended Next Steps
1. Add test files: tests/auth.test.ts, tests/users.test.ts
2. Run: npm test -- --coverage

> Please review this test strategy. Reply APPROVE, then run /test-copilot:generate-tests.
```

**Step 2** — after you reply `APPROVE`, `/test-copilot:generate-tests` writes the actual `tests/*.test.ts` files using Jest + Supertest and proposes a CI step.
