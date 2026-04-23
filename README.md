# GLM Test Engineer Plugin

Test engineering agent for creating, reviewing, and improving test strategies and test cases for any type of software project.

Attention:

- This plugin is designed to work specifically with the GLM Coding Plan in Claude Code.
- This plugin extends Claude Code with a specialized test engineering agent.

## How to use

In Claude Code, run:
```
/glm-test-engineer:test-engineer
```

## Command overview

### /test-engineer

Create, review, or improve test strategies and test cases.

**Execution flow:**
1. Command `/test-engineer` triggers `@test-engineer-agent`
2. The agent analyzes the codebase and project type
3. The agent generates comprehensive test strategies and test cases
4. The agent provides actionable, specific test recommendations

**Specializations:**
- REST API Testing (contract testing, rate limiting, security headers)
- GraphQL Testing (query complexity, nested resolvers, schema integrity)
- Mobile Application Testing (network conditions, device fragmentation, lifecycle)
- Web Application Testing (cross-browser, responsive design, accessibility)
- Microservices Testing (circuit breakers, service discovery, distributed tracing)
