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
2. The agent scans the codebase: detects project type, test framework, and inventories existing tests
3. The agent invokes the test-engineer-skill for deep testing knowledge
4. The agent generates a complete test strategy and self-checks output quality before delivering

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
