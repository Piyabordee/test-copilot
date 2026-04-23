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
2. The agent analyzes the codebase and detects the project type
3. The agent invokes the test-engineer-skill for deep testing knowledge
4. The agent generates comprehensive test strategies with specific, implementable test cases

**Specializations:**
- REST API Testing (contract testing, rate limiting, security headers)
- GraphQL Testing (query complexity, nested resolvers, schema integrity)
- Mobile Application Testing (network conditions, device fragmentation, lifecycle)
- Web Application Testing (cross-browser, responsive design, accessibility)
- Microservices Testing (circuit breakers, service discovery, distributed tracing)
- Security Testing (OWASP Top 10 injection, auth, authorization, data exposure)
- Performance Testing (response time percentiles, throughput, load testing)

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

### Suite: Auth API (/api/auth)
| # | Test Case | Method | Input | Expected | Priority |
|---|-----------|--------|-------|----------|----------|
| 1 | Login with valid credentials | POST | {email, password} | 200 + JWT | Critical |
| 2 | Login with wrong password | POST | {email, "wrong"} | 401 | Critical |
| 3 | Rate limit after 5 failures | POST | 6x failed login | 429 | Critical |

### Security Tests
- [ ] SQL injection on login email field
- [ ] JWT expiry validation
- [ ] No PII in error responses

### Recommended Next Steps
1. Add test files: tests/auth.test.ts, tests/users.test.ts
2. Run: npm test -- --coverage
```
