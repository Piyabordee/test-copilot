# Test Engineer Skill

The domain knowledge base for the Test Copilot agents.

## What It Contains

- **Project Type Detection** — Glob/Grep patterns to identify Node.js, Python, Java, Mobile, and Microservices projects
- **Test Framework Mapping** — Auto-detects existing frameworks or recommends the right one for your stack
- **5 Conditional Sections** — REST API, GraphQL, Web Application, Mobile Application, Microservices
- **Universal Security Checklist** — Injection, Authentication, Authorization, Data Exposure, Rate Limiting
- **Performance Benchmarks** — p50/p95/p99 targets, throughput, CPU/memory utilization
- **Risk-Based Prioritization** — Critical/High/Medium/Low with minimum coverage targets
- **Few-Shot Output Templates** — Tabular format for API tests, BDD-Gherkin for web tests

## How It Works

This skill is invoked automatically by both `test-engineer-agent` (when producing a test strategy) and `test-codegen-agent` (when generating code). You do not need to invoke it directly — use the `/test-copilot:test-engineer` and `/test-copilot:generate-tests` commands instead.

## Related Files

- `agents/test-engineer-agent.md` — Strategy agent (step 1) that calls this skill
- `agents/test-codegen-agent.md` — Codegen agent (step 2) that calls this skill
- `commands/test-engineer.md` — Strategy slash command (step 1)
- `commands/generate-tests.md` — Codegen slash command (step 2)
