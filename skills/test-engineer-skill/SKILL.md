---
name: test-engineer-skill
description: Deep test engineering knowledge for creating, reviewing, and improving test strategies and test cases. Only use when invoked by test-engineer-agent.
allowed-tools: Bash, Read, Glob, Grep
---

# Test Engineer Skill

## Critical Constraints

- Every test case MUST be immediately implementable — no vague descriptions, every assertion must be specific
- NEVER recommend `sleep()` or static waits — always use explicit waits, polling, or event-driven approaches
- Treat flaky tests as P0 bugs — they reveal real race conditions or timing issues in production code
- Test data setup belongs in BeforeEach/fixture setup, NEVER in teardown — faster failure detection
- Every test MUST run independently with no shared state or execution order dependency

## Analysis Workflow

Follow these steps in order when invoked:

### Step 1: Detect Project Type

Scan the codebase using Glob and Grep to identify the project type:

```
Glob patterns to check:
- package.json, tsconfig.json → Node.js project
- requirements.txt, setup.py, pyproject.toml → Python project
- pom.xml, build.gradle → Java project
- *.xcodeproj, *.xcworkspace, AndroidManifest.xml → Mobile project
- Dockerfile, docker-compose.yml, k8s/ → Microservices/containers

Grep patterns to detect framework:
- "express|fastify|koa|hono" → REST API
- "graphql|apollo|type-graphql" → GraphQL
- "next|nuxt|remix|vite|react|vue|angular" → Web Application
- "react-native|flutter|swiftui|jetpack" → Mobile Application
- "kafka|rabbitmq|grpc|nats" → Microservices/Event-driven
```

You may detect multiple types — apply all relevant sections below.

### Step 2: Identify Test Framework

Map the detected tech stack to the appropriate test framework:

| Tech Stack | Test Framework | Assertion Library |
|-----------|---------------|-------------------|
| Node.js/TypeScript | Jest / Vitest | built-in |
| Python | Pytest | built-in |
| Java | JUnit 5 | AssertJ |
| Go | testing (stdlib) | testify |
| React/Vue frontend | Cypress / Playwright | built-in |
| React Native | Detox / Maestro | built-in |
| Flutter | integration_test | built-in |
| .NET | xUnit / NUnit | FluentAssertions |

If a test framework is already configured in the project, USE IT — do not recommend a different one.

### Step 3: Apply Relevant Sections

Read the conditional sections below that match the detected project type. Skip sections that don't apply.

### Step 4: Generate Output

Use the Few-Shot Output Templates at the bottom to format your response.

---

## Conditional Sections

---

### Section A: REST API Testing

**Use when:** Express, Fastify, Koa, Hono, Django REST, Flask, Spring Boot, ASP.NET detected

**Test Categories (implement ALL):**

1. **Contract Tests** — Verify request/response schema for every endpoint

```
Test case format:
| Endpoint | Method | Headers | Request Body | Expected Response | Status Code |
|----------|--------|---------|-------------|-------------------|-------------|
| /api/users | POST | Authorization: Bearer {token} | {"name":"test","email":"test@example.com"} | {"id":1,"name":"test","email":"test@example.com"} | 201 |
| /api/users | POST | (no auth) | {"name":"test","email":"test@example.com"} | {"error":"Unauthorized"} | 401 |
| /api/users | POST | Authorization: Bearer {token} | {"name":""} | {"error":"name is required"} | 400 |
```

2. **Pagination Tests** — Verify limit/offset or cursor-based pagination
   - Request beyond available data returns empty array, not 404
   - Default pagination values are applied when omitted
   - Negative or zero limit values are rejected with 400

3. **Filtering & Sorting Tests** — Every filter parameter, every sort direction
   - Invalid filter values return 400 with descriptive error
   - Sort by non-existent field returns 400, not empty result

4. **Error Handling Tests**
   - 400: Invalid input, missing required fields, type mismatch
   - 401: Missing token, expired token, invalid token format
   - 403: Valid token but insufficient permissions
   - 404: Resource not found (differentiate from deleted resource)
   - 409: Duplicate creation, concurrent modification
   - 429: Rate limit exceeded — verify Retry-After header
   - 500: Unexpected server error — verify no stack trace or PII in response

5. **Security Header Tests**
   ```
   Verify response headers on ALL endpoints:
   - X-Content-Type-Options: nosniff
   - X-Frame-Options: DENY
   - Strict-Transport-Security: max-age=...
   - Content-Security-Policy: (appropriate policy)
   - X-Request-ID: (present and unique)
   ```

---

### Section B: GraphQL Testing

**Use when:** Apollo, TypeGraphQL, GraphQL.js, Hasura, Strawberry detected

**Test Categories (implement ALL):**

1. **Query Depth & Complexity Tests**
   - Query nesting beyond max depth returns error with depth limit message
   - Complex query exceeding complexity budget returns error
   - Introspection query is disabled in production (test both environments)

2. **Resolver Tests**
   - Nested resolver returns data only for authorized fields
   - Null propagation works correctly at every nesting level
   - N+1 query detection — verify DataLoader/batching is used

3. **Mutation Side Effect Tests**
   - Mutation returns correct type on success
   - Mutation rolls back all changes on partial failure
   - Concurrent identical mutations are idempotent

4. **Schema Integrity Tests**
   - Deprecated fields still work but emit deprecation warning
   - Schema evolution: new required fields have defaults for existing clients
   - Union types resolve to correct concrete types

---

### Section C: Web Application Testing

**Use when:** React, Vue, Angular, Next.js, Nuxt, Remix, Svelte detected

**Test Categories (implement ALL):**

**Format: Use BDD-Gherkin (Given-When-Then)**

```
Example:
Feature: User Login

  Scenario: Successful login with valid credentials
    Given the user is on the login page
    And the user has a valid account with email "user@example.com"
    When the user enters "user@example.com" in the email field
    And the user enters "correctpassword" in the password field
    And the user clicks the "Sign In" button
    Then the user should be redirected to the dashboard
    And the navigation bar should display the user's name

  Scenario: Login fails with incorrect password
    Given the user is on the login page
    When the user enters "user@example.com" in the email field
    And the user enters "wrongpassword" in the password field
    And the user clicks the "Sign In" button
    Then the page should display "Invalid email or password"
    And the password field should be cleared
    And the user should remain on the login page
```

**Must-test areas:**

1. **Cross-Browser** — Run core user journeys on Chrome, Firefox, Safari, Edge (minimum)
2. **Responsive Breakpoints** — Test at 375px (mobile), 768px (tablet), 1280px (desktop), 1920px (wide)
3. **Accessibility (WCAG 2.1 AA)**
   - All interactive elements reachable via keyboard (Tab order)
   - Screen reader announces all dynamic content changes
   - Color contrast ratio ≥ 4.5:1 for text, ≥ 3:1 for large text
   - Every `<img>` has meaningful `alt` text
   - Form inputs have associated `<label>` elements
4. **Session Management**
   - Session expires after timeout — user redirected to login
   - Concurrent sessions handled per business rule (allow or revoke old)
   - CSRF token validated on state-changing requests

---

### Section D: Mobile Application Testing

**Use when:** React Native, Flutter, Swift, Kotlin, Android detected

**Test Categories (implement ALL):**

1. **Network Conditions Matrix**

```
Test across:
| Condition | Latency | Bandwidth | Packet Loss |
|-----------|---------|-----------|-------------|
| Offline | ∞ | 0 | 100% |
| Poor 2G | 2000ms | 50 Kbps | 5% |
| Good 3G | 500ms | 750 Kbps | 1% |
| WiFi | 50ms | 10 Mbps | 0% |

For each condition verify:
- App shows appropriate offline/loading indicator
- Failed requests are queued and retried when connection restored
- No data corruption on interrupted transfers
```

2. **App Lifecycle Tests**
   - App state preserved when going to background and returning
   - App recovers gracefully after being killed by OS (low memory)
   - Deep link navigates to correct screen regardless of app state (cold/warm/hot start)

3. **Device Matrix Priority** (test top 80% of user base)

```
Priority order:
1. Latest 2 OS versions (iOS 17-18, Android 14-15)
2. Most common screen sizes (iPhone 15, Samsung Galaxy S24, etc.)
3. Minimum supported OS version (ensure app doesn't crash on old devices)
```

4. **Push Notification Tests**
   - Notification received in foreground, background, and killed states
   - Tapping notification navigates to correct deep-linked screen
   - Badge count updates correctly

---

### Section E: Microservices Testing

**Use when:** Docker, Kubernetes, Kafka, RabbitMQ, gRPC, NATS, service mesh detected

**Test Categories (implement ALL):**

1. **Contract Testing (Consumer-Driven)**
   - Each consumer publishes expected contract
   - Provider verified against all consumer contracts
   - Contract tests run in CI before deployment — failing contract = block deploy

2. **Failure Mode Tests**
   - Dependency service down → circuit breaker opens within timeout threshold
   - Circuit breaker provides meaningful fallback response
   - Retired service endpoint returns 410 Gone, not 500
   - Timeout handling: request fails gracefully at configured timeout, not hanging

3. **Event/Message Tests**
   - Event published in exactly-once semantics (no duplicates after retry)
   - Event consumer is idempotent — processing same event twice = same result
   - Dead letter queue receives events that fail after max retries
   - Event schema changes are backward compatible

4. **Distributed Tracing**
   - Trace ID propagates across all service boundaries
   - Span duration measured at each hop
   - Failed spans tagged with error type and HTTP status

---

## Universal Security Checklist

**Apply to ALL project types.** Test every item below:

### Injection
- SQL: `' OR 1=1 --`, `"; DROP TABLE users; --` → expect 400 or sanitized input
- NoSQL: `{"$gt": ""}`, `{"$where": "sleep(5000)"}` → expect rejection
- Command: `; rm -rf /`, `$(cat /etc/passwd)` → expect sanitized or rejected
- LDAP: `*)(objectClass=*`, `*)(uid=*))(|(uid=*` → expect rejection

### Authentication
- JWT with expired `exp` claim → expect 401
- JWT with tampered signature → expect 401
- JWT with valid signature but revoked `jti` → expect 401
- Password in response body → MUST NOT appear (even masked)
- Token in URL query parameter → MUST NOT appear (use header instead)

### Authorization
- User with Role A accessing Resource B (restricted to Role C) → expect 403
- Horizontal access: User A accessing User B's resource → expect 403
- Vertical access: Regular user accessing admin endpoint → expect 403
- API key with expired `expires_at` → expect 401

### Data Exposure
- Error response contains stack trace → MUST NOT appear in production
- Error response contains database query → MUST NOT appear
- Response includes PII in logs → MUST NOT appear
- Pagination response leaks total user count → verify if intentional

### Rate Limiting
- Send N+1 requests where N = rate limit → expect 429 on request N+1
- Verify `Retry-After` header is present on 429 response
- Verify rate limit resets correctly after window expires

---

## Performance Testing Benchmarks

**Define targets before testing:**

```
Template:
| Metric | Target | Measurement |
|--------|--------|-------------|
| p50 Response Time | ≤ 200ms | API endpoints |
| p95 Response Time | ≤ 500ms | API endpoints |
| p99 Response Time | ≤ 1000ms | API endpoints |
| Throughput | ≥ {N} req/s | Sustained load |
| Error Rate | ≤ 0.1% | Under normal load |
| CPU Utilization | ≤ 70% | Under peak load |
| Memory Utilization | ≤ 80% | Steady state |
| DB Connection Pool | ≤ 80% used | Under peak load |

Stress test: Double expected peak load → verify graceful degradation, not crash
```

---

## Risk-Based Test Prioritization

**Use this to decide test execution order:**

| Priority | Areas | Minimum Coverage Target |
|----------|-------|------------------------|
| **Critical** | Authentication, payment, data deletion, security controls, PII handling | 100% — every path tested |
| **High** | User data modification, bulk operations, external integrations, data persistence | ≥ 90% |
| **Medium** | Read operations, search/filter, reporting, non-sensitive data retrieval | ≥ 80% |
| **Low** | Static content, cosmetic UI, optional features, admin-only settings | ≥ 60% |

---

## Few-Shot Output Templates

### Template 1: API Test Strategy Output

```
# Test Strategy: {Project Name}

## Detected Project Type
- Primary: REST API (Express.js)
- Secondary: Web Application (React)
- Test Framework: Jest + Supertest

## Test Plan Summary
- Total test suites: 12
- Total test cases: 87
- Estimated execution time: ~4 minutes (parallel)

## Test Cases

### Suite: Auth API (`/api/auth`)
| # | Test Case | Method | Endpoint | Input | Expected | Status |
|---|-----------|--------|----------|-------|----------|--------|
| 1 | Login with valid credentials | POST | /api/auth/login | {email, password} | 200 + JWT token | Critical |
| 2 | Login with wrong password | POST | /api/auth/login | {email, "wrong"} | 401 + error message | Critical |
| 3 | Login with malformed email | POST | /api/auth/login | {email: "notemail"} | 400 + validation error | High |
| ... | ... | ... | ... | ... | ... | ... |

### Security Tests
- [ ] SQL injection on login email field
- [ ] Rate limit: >5 failed logins in 15 min → 429
- [ ] JWT expiry validation

### Recommended Next Steps
1. Add these test files: `tests/auth.test.ts`, `tests/users.test.ts`
2. Run `npm test -- --coverage` to verify coverage ≥ 80%
3. Add CI pipeline step: `npm run test:integration`
```

### Template 2: Web Application Test Strategy Output

```
# Test Strategy: {Project Name}

## Detected Project Type
- Primary: Web Application (Next.js + React)
- Test Framework: Playwright + Jest

## BDD Test Scenarios

### Feature: User Registration

  Scenario: Register with valid information
    Given the user is on "/register"
    When the user fills in "email" with "new@example.com"
    And the user fills in "password" with "Str0ng!Pass"
    And the user fills in "confirmPassword" with "Str0ng!Pass"
    And the user clicks "Create Account"
    Then the user should be redirected to "/verify-email"
    And a verification email should be sent to "new@example.com"

  Scenario: Register with existing email
    Given the user is on "/register"
    And a user with email "existing@example.com" already exists
    When the user fills in "email" with "existing@example.com"
    And the user fills in "password" with "Str0ng!Pass"
    And the user clicks "Create Account"
    Then the page should display "An account with this email already exists"
    And no new account should be created

### Accessibility Tests
- [ ] Tab order follows logical reading sequence on registration form
- [ ] All form inputs have visible labels
- [ ] Error messages are announced by screen reader (aria-live)
- [ ] Color contrast of error text ≥ 4.5:1

### Cross-Browser Matrix
| Browser | Version | Status |
|---------|---------|--------|
| Chrome | 120+ | Required |
| Firefox | 121+ | Required |
| Safari | 17+ | Required |
| Edge | 120+ | Required |

### Recommended Next Steps
1. Create Playwright tests in `e2e/registration.spec.ts`
2. Run `npx playwright test --reporter=html`
3. Add accessibility audit: `npx axe-core --include=register`
```

---

## Quality Constraints

- If project type is unclear after analysis, EXPLICITLY state assumptions before generating — do not guess silently
- Every recommended test case MUST include: what to test, expected input, expected outcome, priority level
- When recommending a test framework different from what's installed, provide the migration reason and installation command
- If the codebase already has tests, REVIEW them first before generating new ones — extend, don't duplicate
- Always provide a "Recommended Next Steps" section with specific file paths and commands to execute
