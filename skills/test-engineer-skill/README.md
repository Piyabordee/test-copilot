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

This skill is invoked automatically by the `test-engineer-agent` during Phase 2 of its execution protocol. You do not need to invoke it directly — use the `/test-copilot:test-engineer` command instead.

## Related Files

- `agents/test-engineer-agent.md` — The orchestrating agent that calls this skill
- `commands/test-engineer.md` — The slash command entry point
