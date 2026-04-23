---
name: test-engineer-skill
description: Test engineering skill for creating, reviewing, and improving test strategies and test cases. Only use when invoked by test-engineer-agent.
allowed-tools: Bash, Read, Glob, Grep
---

# Test Engineer Skill

This skill provides test engineering capabilities to the test-engineer-agent.

## Usage

This skill is automatically invoked by the test-engineer-agent when test engineering tasks are required.

## Capabilities

- Analyze codebase structure to identify project types (REST API, GraphQL, Mobile, Web, Microservices)
- Generate appropriate test strategies based on project type
- Create test cases in optimal formats (API tables, BDD-Gherkin, structured lists)
- Incorporate security testing patterns (OWASP Top 10)
- Design performance test benchmarks
- Provide risk-based test prioritization

## Tools Available

- **Bash**: Execute test scripts, run test frameworks
- **Read**: Analyze source code and test files
- **Glob**: Find test files and source files
- **Grep**: Search for test patterns and code patterns
