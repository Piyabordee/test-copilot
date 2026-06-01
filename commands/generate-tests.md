---
allowed-tools: Bash, Read, Write, Edit, Skill, Glob, Grep
description: Turn an APPROVED test strategy into executable test code and CI config (step 2 of 2 — writes files).
---

# Generate Tests — Code (Step 2 of 2)

Invoke @test-copilot:test-codegen-agent to turn the **approved** test strategy from this conversation into executable test code and (optionally) CI configuration.

This is the **execute** half of the AI-DLC workflow (Generate → Human Validate → Execute).

**Precondition:** A test strategy produced by `/test-copilot:test-engineer` exists in this conversation and the user has replied `APPROVE`. If no approved strategy is present, the agent will ask you to run `/test-copilot:test-engineer` first.

What it does:

1. Reuses the existing/detected test framework — it does NOT swap frameworks.
2. Generates executable tests that mirror the approved cases (resilient locators, no hard-coded sleeps, isolated tests).
3. Writes AI-generated tests into a clearly labeled location so human-written and AI-drafted tests stay distinguishable.
4. If the project has no CI for tests, proposes a pipeline (and writes it on request), including secrets/environment handling.

> This command writes files. Review the generated code and run it before merging — AI drafts, humans approve.
