# Test Copilot Plugin

AI-powered test engineering agent for creating, reviewing, and improving test strategies and test cases.

**Repository:** https://github.com/Piyabordee/test-copilot

## Architecture

This plugin uses an AI-DLC (Generate → Human Validate → Execute) pipeline, split into two approval-gated steps:

```
Step 1 — Strategy (read-only, never writes files):
  Command (commands/test-engineer.md)
    -> Agent (agents/test-engineer-agent.md) — produces strategy, then STOPS for review
      -> Skill (skills/test-engineer-skill/SKILL.md) — domain knowledge, templates, checklists

   ── human reviews, replies APPROVE ──

Step 2 — Code (writes files):
  Command (commands/generate-tests.md)
    -> Agent (agents/test-codegen-agent.md) — turns approved strategy into runnable code + CI
      -> Skill (skills/test-engineer-skill/SKILL.md) — Executable Code Generation section
```

The two agents enforce the "AI is the intern, the human is the senior" rule: the strategy agent has **no file-writing tools** and must stop for human approval; only the codegen agent (run after approval) can write files.

## Repository Structure

```
test-copilot/
├── .claude-plugin/
│   ├── plugin.json          — Plugin identity and metadata
│   └── marketplace.json     — Marketplace listing for discovery
├── agents/
│   ├── test-engineer-agent.md  — Strategy agent (read-only): plan + STOP for approval
│   └── test-codegen-agent.md   — Codegen agent (writes files): approved strategy → code + CI
├── commands/
│   ├── test-engineer.md        — Slash command: /test-copilot:test-engineer (step 1)
│   └── generate-tests.md       — Slash command: /test-copilot:generate-tests (step 2)
├── skills/
│   └── test-engineer-skill/
│       ├── SKILL.md            — Domain knowledge (strategies, checklists, templates, code generation)
│       └── README.md           — Skill overview for humans
├── CLAUDE.md                   — This file (repo-level instructions)
├── README.md                   — Public documentation
└── INSTALLATION.md             — Installation guide
```

## The Agents

### test-engineer-agent (Strategy — Step 1)

Read-only. Tools: `Bash, Read, Skill, Glob, Grep` (no Write/Edit by design).

1. **Reconnaissance & Context** — Gathers business context (README/docs/PR), scans structure, detects framework, inventories existing tests
2. **Invoke Skill** — Loads domain knowledge from test-engineer-skill
3. **Generate Strategy** — Produces complete test strategy with security checklist and benchmarks
4. **Self-Check** — Verifies all required elements
5. **STOP & Request Approval** — Asks the user to review and reply `APPROVE`; never generates code or writes files

### test-codegen-agent (Code — Step 2)

Writes files. Tools: `Bash, Read, Write, Edit, Skill, Glob, Grep`.

Runs only after the user approves the strategy. Reuses the existing test framework (never swaps it), generates runnable tests that mirror the approved cases, writes them to a clearly labeled location (e.g. `tests/e2e/ai-generated/`), and conditionally proposes a CI pipeline with secrets/environment handling.

## Skills

### test-engineer-skill

Deep test engineering knowledge covering:
- REST API Testing (contract, pagination, error handling, security headers)
- GraphQL Testing (query depth, resolvers, mutations, schema integrity)
- Web Application Testing (BDD-Gherkin, cross-browser, accessibility WCAG 2.1 AA)
- Mobile Application Testing (network matrix, lifecycle, device fragmentation)
- Microservices Testing (contract testing, circuit breakers, event idempotency)
- Universal Security Checklist (OWASP Top 10)
- Performance Testing Benchmarks (response time, throughput, resource utilization)
- Executable Code Generation (framework-agnostic codegen, Playwright best practices, CI/CD, secrets/env)

## Working with This Repository

When modifying this plugin:

1. **Adding test categories** — Add new conditional sections to `skills/test-engineer-skill/SKILL.md` and update the strategy agent's Phase 3 if needed
2. **Changing strategy output format** — Edit the Few-Shot Output Templates in the skill and the Self-Check in `test-engineer-agent.md`
3. **Changing code generation** — Edit the Executable Code Generation section in the skill and the Self-Check in `test-codegen-agent.md`
4. **Preserving the approval gate** — The strategy agent must never gain Write/Edit tools or skip its STOP step; file writes belong only to the codegen agent after `APPROVE`
5. **Adding new skills** — Create `skills/<name>/SKILL.md` with YAML frontmatter (`name`, `description`), then update `marketplace.json` keywords
6. **Testing changes** — Run `/test-copilot:test-engineer` in a sample project, approve, then `/test-copilot:generate-tests`, and verify both steps
