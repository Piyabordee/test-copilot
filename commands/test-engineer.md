---
allowed-tools: Bash, Read, Skill, Glob, Grep
description: Analyze the codebase and produce a test strategy and test cases for human review (step 1 of 2 — does not write code).
---

# Test Engineer — Strategy (Step 1 of 2)

Invoke @test-copilot:test-engineer-agent to analyze the codebase and produce a comprehensive **test strategy and test cases** for human review.

This is the **plan** half of an AI-DLC workflow (Generate → Human Validate → Execute):

1. The agent gathers business context, detects project type & test framework, and inventories existing tests.
2. It invokes `test-engineer-skill` for deep testing knowledge.
3. It produces a complete strategy (test-case tables / BDD scenarios, security checklist, performance benchmarks, risk priorities) and self-checks it.
4. It **STOPS and asks you to review.** Iterate until you are satisfied, then reply `APPROVE`.

To turn the approved strategy into executable test code, run `/test-copilot:generate-tests`.

This agent specializes in:
- Writing API test suites (REST/GraphQL)
- Creating end-to-end tests for web/mobile applications
- Designing integration tests for microservices
- Implementing security testing scenarios
- Establishing testing frameworks
- Optimizing test coverage
- Debugging flaky tests

> This command never writes files. Code generation is handled by `/test-copilot:generate-tests` after approval.
