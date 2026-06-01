# Changelog

All notable changes to the Test Copilot plugin.

---

## [0.2.0] — HITL 2-step

### 🏗️ Architecture: Human-in-the-Loop, 2-Step Pipeline

The plugin is rebuilt around an **AI-DLC** workflow — **Generate → Human Validate → Execute** — split into two approval-gated steps so AI drafts and humans approve before any code is written.

```
Step 1 — Strategy (read-only, never writes files):
  /test-copilot:test-engineer
    → test-engineer-agent  — produces strategy, then STOPS for review

   ── human reviews, replies APPROVE ──

Step 2 — Code (writes files):
  /test-copilot:generate-tests
    → test-codegen-agent   — turns approved strategy into runnable code + CI
```

### ✨ Added

- **Two-agent architecture** — Strategy agent (read-only) and Codegen agent (writes files), enforcing "AI is the intern, human is the senior"
- **Approval gate** — Strategy agent stops and waits for human `APPROVE` before any file is written
- **`/test-copilot:generate-tests`** command — Step 2: turns approved strategy into runnable tests + CI pipeline proposal
- **Marketplace compatibility** — `.claude-plugin/marketplace.json` and `plugin.json` for `GitHub: Piyabordee/test-copilot` one-line install
- **`CLAUDE.md`** — Repo-level instructions for Claude Code auto-discovery
- **`INSTALLATION.md`** — Step-by-step installation guide (marketplace + manual)
- **Skill README** — `skills/test-engineer-skill/README.md` for human overview

### 🔄 Changed

- **Renamed from `glm-test-engineer` to `test-copilot`**
- **`test-engineer-agent`** — Now read-only (no Write/Edit tools), stops for approval, won't generate code
- **`skills/test-engineer-skill/SKILL.md`** — Added keyword-dense "Use when..." frontmatter for better auto-activation
- **`README.md`** — Marketplace-first installation, 2-step usage guide

### 🗑️ Removed

- **Single-agent mode** — Replaced by the 2-step HITL pipeline
- **Empty `scripts/` directory** under the skill

---

## [0.1.0] — Initial Release

### ✨ Added

- **`/test-engineer`** slash command — Generate test strategies and test cases
- **`test-engineer-agent`** — Single agent for test strategy generation
- **`test-engineer-skill`** — Deep test engineering domain knowledge covering:
  - REST API Testing (contract, pagination, error handling, security headers)
  - GraphQL Testing (query depth, resolvers, mutations, schema integrity)
  - Web Application Testing (BDD-Gherkin, cross-browser, accessibility WCAG 2.1 AA)
  - Mobile Application Testing (network matrix, lifecycle, device fragmentation)
  - Microservices Testing (contract testing, circuit breakers, event idempotency)
  - Universal Security Checklist (OWASP Top 10)
  - Performance Testing Benchmarks
- **Project scaffolding** — `plugin.json`, agent/command/skill structure
