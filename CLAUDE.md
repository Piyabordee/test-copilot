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
