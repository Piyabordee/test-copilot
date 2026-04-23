# Marketplace-Compatible Migration Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Migrate the glm-test-engineer plugin to be marketplace-compatible so users can install via `GitHub: Piyabordee/glm-test-engineer` instead of manual file copying.

**Architecture:** Add `.claude-plugin/marketplace.json` and update `plugin.json` to match the Claude Code marketplace schema. Create `CLAUDE.md` as repo-level instructions. Update SKILL.md frontmatter with keyword-dense "Use when..." description for better auto-activation. All existing functionality (command -> agent -> skill pipeline) is preserved exactly as-is.

**Tech Stack:** Claude Code plugin system (YAML frontmatter, JSON manifests, Markdown)

---

## File Structure

| File | Action | Responsibility |
|------|--------|----------------|
| `.claude-plugin/plugin.json` | Modify | Full metadata with keywords, repository, homepage, license |
| `.claude-plugin/marketplace.json` | Create | Marketplace listing entry pointing to `"./"` |
| `CLAUDE.md` | Create | Repo-level instructions for Claude Code auto-discovery |
| `skills/test-engineer-skill/SKILL.md` | Modify | Keyword-dense frontmatter description for activation triggering |
| `skills/test-engineer-skill/README.md` | Create | Brief skill overview for humans |
| `README.md` | Modify | Add marketplace installation as primary method |
| `INSTALLATION.md` | Modify | Add marketplace method at top, keep manual as fallback |
| `skills/test-engineer-skill/scripts/` | Delete | Remove empty directory |

**Unchanged files (preserved exactly):**
- `agents/test-engineer-agent.md` — agent orchestration logic
- `commands/test-engineer.md` — slash command entry point

---

### Task 1: Update `.claude-plugin/plugin.json` with full metadata

**Files:**
- Modify: `.claude-plugin/plugin.json`

- [ ] **Step 1: Replace plugin.json with full metadata**

```json
{
  "name": "glm-test-engineer",
  "version": "0.1.0",
  "description": "AI-powered test engineering agent for creating, reviewing, and improving test strategies and test cases for any software project",
  "author": {
    "name": "Piyabordee",
    "url": "https://github.com/Piyabordee"
  },
  "license": "MIT",
  "keywords": [
    "testing",
    "qa",
    "test-strategy",
    "test-cases",
    "rest-api",
    "graphql",
    "web-testing",
    "mobile-testing",
    "microservices",
    "security-testing",
    "owasp",
    "performance-testing",
    "bdd",
    "gherkin"
  ],
  "repository": "https://github.com/Piyabordee/glm-test-engineer",
  "homepage": "https://github.com/Piyabordee/glm-test-engineer"
}
```

- [ ] **Step 2: Verify JSON is valid**

Run: `cat .claude-plugin/plugin.json | python -m json.tool`
Expected: Pretty-printed JSON with no errors

- [ ] **Step 3: Commit**

```bash
git add .claude-plugin/plugin.json
git commit -m "feat: update plugin.json with full marketplace metadata"
```

---

### Task 2: Create `.claude-plugin/marketplace.json`

**Files:**
- Create: `.claude-plugin/marketplace.json`

- [ ] **Step 1: Create marketplace.json**

```json
{
  "name": "glm-test-engineer",
  "description": "AI-powered test engineering agent for creating, reviewing, and improving test strategies and test cases",
  "owner": {
    "name": "Piyabordee",
    "url": "https://github.com/Piyabordee"
  },
  "plugins": [
    {
      "name": "glm-test-engineer",
      "source": "./",
      "description": "AI-powered test engineering agent that analyzes your codebase, detects project type and test framework, then generates comprehensive test strategies with security checklists, performance benchmarks, and risk-based prioritization. Supports REST API, GraphQL, Web, Mobile, and Microservices testing.",
      "version": "0.1.0",
      "author": {
        "name": "Piyabordee",
        "url": "https://github.com/Piyabordee"
      },
      "category": "development",
      "keywords": [
        "testing",
        "qa",
        "test-strategy",
        "test-cases",
        "rest-api",
        "graphql",
        "web-testing",
        "mobile-testing",
        "microservices",
        "security-testing",
        "owasp",
        "performance-testing",
        "bdd",
        "gherkin"
      ],
      "homepage": "https://github.com/Piyabordee/glm-test-engineer",
      "repository": "https://github.com/Piyabordee/glm-test-engineer",
      "license": "MIT"
    }
  ]
}
```

- [ ] **Step 2: Verify JSON is valid**

Run: `cat .claude-plugin/marketplace.json | python -m json.tool`
Expected: Pretty-printed JSON with no errors

- [ ] **Step 3: Commit**

```bash
git add .claude-plugin/marketplace.json
git commit -m "feat: add marketplace.json for Claude Code marketplace discovery"
```

---

### Task 3: Update SKILL.md frontmatter with keyword-dense description

**Files:**
- Modify: `skills/test-engineer-skill/SKILL.md`

- [ ] **Step 1: Replace the YAML frontmatter**

Change from:
```yaml
---
name: test-engineer-skill
description: Deep test engineering knowledge for creating, reviewing, and improving test strategies and test cases. Only use when invoked by test-engineer-agent.
allowed-tools: Bash, Read, Glob, Grep
---
```

To:
```yaml
---
name: test-engineer-skill
description: Comprehensive test engineering knowledge base for creating, reviewing, and improving test strategies and test cases. Use when writing test cases, designing test strategies, performing security testing, checking OWASP Top 10 vulnerabilities, generating REST API contract tests, writing BDD-Gherkin scenarios for web applications, testing GraphQL queries and mutations, creating mobile app test matrices, testing microservices with circuit breakers and event-driven patterns, setting performance benchmarks, prioritizing tests by risk level, detecting project type for test framework selection, or reviewing existing test coverage. Invoked by test-engineer-agent during test strategy generation.
---
```

**Why this change:** The original description was too short and restrictive ("Only use when invoked by test-engineer-agent"). The n8n-skills pattern uses long, keyword-dense descriptions with "Use when..." triggers. This improves Claude Code's skill matching. The `allowed-tools` field is removed because tools are specified at the agent/command level, not the skill level in marketplace-compatible plugins.

- [ ] **Step 2: Verify the file starts correctly**

Run: `head -6 skills/test-engineer-skill/SKILL.md`
Expected: The new frontmatter with `---`, `name:`, `description:`, `---`, followed by a blank line and `# Test Engineer Skill`

- [ ] **Step 3: Commit**

```bash
git add skills/test-engineer-skill/SKILL.md
git commit -m "feat: update SKILL.md frontmatter with keyword-dense activation triggers"
```

---

### Task 4: Create skill README for human readers

**Files:**
- Create: `skills/test-engineer-skill/README.md`

- [ ] **Step 1: Create the file**

```markdown
# Test Engineer Skill

The domain knowledge base for the GLM Test Engineer agent.

## What It Contains

- **Project Type Detection** — Glob/Grep patterns to identify Node.js, Python, Java, Mobile, and Microservices projects
- **Test Framework Mapping** — Auto-detects existing frameworks or recommends the right one for your stack
- **5 Conditional Sections** — REST API, GraphQL, Web Application, Mobile Application, Microservices
- **Universal Security Checklist** — Injection, Authentication, Authorization, Data Exposure, Rate Limiting
- **Performance Benchmarks** — p50/p95/p99 targets, throughput, CPU/memory utilization
- **Risk-Based Prioritization** — Critical/High/Medium/Low with minimum coverage targets
- **Few-Shot Output Templates** — Tabular format for API tests, BDD-Gherkin for web tests

## How It Works

This skill is invoked automatically by the `test-engineer-agent` during Phase 2 of its execution protocol. You do not need to invoke it directly — use the `/glm-test-engineer:test-engineer` command instead.

## Related Files

- `agents/test-engineer-agent.md` — The orchestrating agent that calls this skill
- `commands/test-engineer.md` — The slash command entry point
```

- [ ] **Step 2: Commit**

```bash
git add skills/test-engineer-skill/README.md
git commit -m "feat: add skill README for human readers"
```

---

### Task 5: Create CLAUDE.md at repo root

**Files:**
- Create: `CLAUDE.md`

- [ ] **Step 1: Create CLAUDE.md**

```markdown
# GLM Test Engineer Plugin

AI-powered test engineering agent for creating, reviewing, and improving test strategies and test cases.

**Repository:** https://github.com/Piyabordee/glm-test-engineer

## Architecture

This plugin uses a 3-layer pipeline:

```
Command (commands/test-engineer.md)
  -> Agent (agents/test-engineer-agent.md) — 4-phase orchestration protocol
    -> Skill (skills/test-engineer-skill/SKILL.md) — Domain knowledge, templates, checklists
```

## Repository Structure

```
glm-test-engineer/
├── .claude-plugin/
│   ├── plugin.json          — Plugin identity and metadata
│   └── marketplace.json     — Marketplace listing for discovery
├── agents/
│   └── test-engineer-agent.md  — Agent: 4-phase QA engineer protocol
├── commands/
│   └── test-engineer.md        — Slash command: /glm-test-engineer:test-engineer
├── skills/
│   └── test-engineer-skill/
│       ├── SKILL.md            — Domain knowledge (testing strategies, checklists, templates)
│       └── README.md           — Skill overview for humans
├── CLAUDE.md                   — This file (repo-level instructions)
├── README.md                   — Public documentation
└── INSTALLATION.md             — Installation guide
```

## The Agent

`test-engineer-agent` follows a strict 4-phase protocol:

1. **Reconnaissance** — Scans project structure, detects framework, inventories existing tests
2. **Invoke Skill** — Loads domain knowledge from test-engineer-skill
3. **Generate Output** — Produces complete test strategy with security checklist and benchmarks
4. **Self-Check** — Verifies all required elements before delivery

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

## Working with This Repository

When modifying this plugin:

1. **Adding test categories** — Add new conditional sections to `skills/test-engineer-skill/SKILL.md` and update the agent's Phase 3 if needed
2. **Changing output format** — Edit the Few-Shot Output Templates in the skill and the Self-Check in the agent
3. **Adding new skills** — Create `skills/<name>/SKILL.md` with YAML frontmatter (`name`, `description`), then update `marketplace.json` keywords
4. **Testing changes** — Run `/glm-test-engineer:test-engineer` in a sample project and verify the 4-phase output
```

- [ ] **Step 2: Commit**

```bash
git add CLAUDE.md
git commit -m "feat: add CLAUDE.md repo-level instructions for Claude Code auto-discovery"
```

---

### Task 6: Update README.md with marketplace installation

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Read current README.md**

Read: `README.md`

- [ ] **Step 2: Replace README.md with marketplace-first version**

```markdown
# GLM Test Engineer

AI-powered test engineering agent for creating, reviewing, and improving test strategies and test cases for any type of software project.

## Installation

### Via Claude Code Marketplace (Recommended)

In Claude Code, type:

```
GitHub: Piyabordee/glm-test-engineer
```

The plugin will be automatically discovered and installed.

### Manual Installation

See [INSTALLATION.md](INSTALLATION.md) for step-by-step manual setup.

## How to Use

In Claude Code, run:

```
/glm-test-engineer:test-engineer
```

## Command Overview

### /test-engineer

Create, review, or improve test strategies and test cases.

**Execution flow:**

1. Command `/test-engineer` triggers `@test-engineer-agent`
2. The agent scans the codebase: detects project type, test framework, and inventories existing tests
3. The agent invokes the `test-engineer-skill` for deep testing knowledge
4. The agent generates a complete test strategy and self-checks output quality before delivering

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
Command → Agent → Skill
   │         │        │
   │         └── 4-phase QA engineer protocol
   │              ├── Phase 1: Reconnaissance (project scan)
   │              ├── Phase 2: Invoke Skill (load knowledge)
   │              ├── Phase 3: Generate Output (strategy + checklist)
   │              └── Phase 4: Self-Check (quality verify)
   │
   └── Entry point (/glm-test-engineer:test-engineer)
```

## Example Output

When you run `/glm-test-engineer:test-engineer` in a Node.js REST API project, you can expect:

```
# Test Strategy: My API Project

## Detected Project Type
- Primary: REST API (Express.js)
- Test Framework: Jest + Supertest

## Test Plan Summary
- Total test suites: 12
- Total test cases: 87

## Test Cases

### Suite: Auth API (`/api/auth`)
| # | Test Case | Method | Endpoint | Input | Expected | Status |
|---|-----------|--------|----------|-------|----------|--------|
| 1 | Login with valid credentials | POST | /api/auth/login | {email, password} | 200 + JWT | Critical |
| 2 | Login with wrong password | POST | /api/auth/login | {email, "wrong"} | 401 + error | Critical |
| 3 | Rate limit after 5 failures | POST | /api/auth/login | 6x failed login | 429 + Retry-After | Critical |

### Security Tests
- [ ] SQL injection on login email field
- [ ] JWT expiry validation
- [ ] No PII in error responses

### Recommended Next Steps
1. Add test files: tests/auth.test.ts, tests/users.test.ts
2. Run: npm test -- --coverage
```
```

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "feat: update README with marketplace installation as primary method"
```

---

### Task 7: Update INSTALLATION.md — add marketplace method at top

**Files:**
- Modify: `INSTALLATION.md`

- [ ] **Step 1: Add marketplace section at the top of INSTALLATION.md**

Insert the following at the very beginning of the file, before the existing `# Test Copilot - Installation Guide` heading:

```markdown
# GLM Test Engineer - Installation Guide

## Quick Install (Recommended)

In Claude Code, type:

```
GitHub: Piyabordee/glm-test-engineer
```

The plugin will be automatically discovered and installed. No manual steps required.

---

## Manual Installation

> Only needed if the marketplace method above doesn't work for your setup.

```

Then change the original `# Test Copilot - Installation Guide` heading to `### Prerequisites` and remove it as a top-level heading.

- [ ] **Step 2: Verify the file reads correctly**

Run: `head -20 INSTALLATION.md`
Expected: Quick Install section first, then "Manual Installation" heading, then the original content

- [ ] **Step 3: Commit**

```bash
git add INSTALLATION.md
git commit -m "feat: add marketplace quick install as primary method in INSTALLATION.md"
```

---

### Task 8: Cleanup — remove empty scripts/ directory

**Files:**
- Delete: `skills/test-engineer-skill/scripts/`

- [ ] **Step 1: Remove empty directory**

```bash
rmdir skills/test-engineer-skill/scripts
```

- [ ] **Step 2: Commit**

```bash
git add -u skills/test-engineer-skill/scripts
git commit -m "chore: remove empty scripts/ directory"
```

---

### Task 9: Final verification — test marketplace detection

**Files:**
- No file changes

- [ ] **Step 1: Verify all required marketplace files exist**

Run: `ls -la .claude-plugin/plugin.json .claude-plugin/marketplace.json CLAUDE.md skills/test-engineer-skill/SKILL.md`
Expected: All 4 files exist

- [ ] **Step 2: Verify plugin.json schema**

Run: `cat .claude-plugin/plugin.json | python -m json.tool`
Expected: Valid JSON with name, version, description, author, license, keywords, repository, homepage

- [ ] **Step 3: Verify marketplace.json schema**

Run: `cat .claude-plugin/marketplace.json | python -m json.tool`
Expected: Valid JSON with plugins array containing one entry with source "./"

- [ ] **Step 4: Verify SKILL.md frontmatter has keyword-dense description**

Run: `head -5 skills/test-engineer-skill/SKILL.md`
Expected: Frontmatter with `name: test-engineer-skill` and long `description:` containing "Use when..."

- [ ] **Step 5: Verify agents and commands are preserved unchanged**

Run: `md5sum agents/test-engineer-agent.md commands/test-engineer.md`
Expected: Files exist and are unchanged from original

- [ ] **Step 6: Verify git status is clean**

Run: `git status`
Expected: Nothing to commit, working tree clean
