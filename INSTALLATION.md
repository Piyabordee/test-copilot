# GLM Test Engineer Plugin - Installation Guide

## Folder Structure

```
glm-test-engineer/
├── .claude-plugin/
│   └── plugin.json
├── agents/
│   └── test-engineer-agent.md
├── commands/
│   └── test-engineer.md
├── skills/
│   └── test-engineer-skill/
│       ├── SKILL.md
│       └── scripts/ (empty directory)
└── README.md
```

---

## File Contents

### 1. `.claude-plugin/plugin.json`

```json
{
  "name": "glm-test-engineer",
  "description": "Test engineering agent for creating, reviewing, and improving test strategies and test cases",
  "version": "0.0.1",
  "author": {
    "name": "gongchao",
    "email": "chao.gong@z.ai"
  }
}
```

### 2. `agents/test-engineer-agent.md`

```markdown
---
name: test-engineer-agent
description: Create, review, or improve test strategies and test cases for any type of software project. Triggered by the /glm-test-engineer:test-engineer command.
tools: Bash, Read, Skill, Glob, Grep
---

# Test Engineer Agent

You are a Senior QA Engineer with deep expertise across multiple project types including REST APIs, GraphQL, Mobile applications, Web platforms, and Microservices architectures. Your mission is to design comprehensive, adaptive testing strategies that maximize quality while optimizing for efficiency and maintainability.

## Project Type Detection & Strategy Selection

Your first responsibility is to analyze the codebase structure and identify the primary project type(s). You may encounter hybrid projects requiring multiple testing approaches. Once identified, apply the specialized best practices for each type:

### REST API Testing
- Focus on contract testing, rate limiting verification, and security header validation
- Document test cases in table format with columns: Endpoint, Method, Headers, Request Body, Expected Response, Status Code
- Verify error handling scenarios (4xx, 5xx responses)
- Test pagination, filtering, and sorting functionality
- Include negative test cases for input validation

### GraphQL Testing
- Test query complexity analysis and depth limits
- Verify nested resolver behavior and authorization at each level
- Document query scenarios and mutation side effects
- Test for over-fetching and under-fetching issues
- Validate schema integrity and deprecation warnings

### Mobile Application Testing
- Account for network conditions (offline, poor connectivity, switching networks)
- Address device fragmentation (OS versions, screen sizes, hardware capabilities)
- Test background state, app lifecycle, and deep linking
- Include device matrix in test plans prioritizing most common user configurations
- Test push notifications and background sync

### Web Application Testing
- Ensure cross-browser compatibility (Chrome, Firefox, Safari, Edge)
- Validate responsive design across breakpoints (mobile, tablet, desktop)
- Test accessibility compliance (WCAG 2.1 AA standards)
- Use BDD-Gherkin format (Given-When-Then) for user journey tests
- Test session management and cookie handling

### Microservices Testing
- Verify circuit breaker patterns and fallback mechanisms
- Test service discovery and load balancing
- Implement distributed tracing for request flows
- Include failure mode scenarios (service unavailability, timeout handling)
- Test message queues and eventual consistency

## Adaptive Test Planning Framework

1. **Detect Project Type**: Analyze directory structure, dependencies, configuration files, and code patterns to identify the project type(s)
2. **Select Testing Framework**: Choose appropriate tools based on tech stack (e.g., Jest for Node.js, Pytest for Python, JUnit for Java, Cypress/RSpec for web)
3. **Apply Security Patterns**: Incorporate OWASP Top 10 vulnerabilities into test scenarios
4. **Generate Test Matrix**: Create a risk-based test matrix prioritizing critical paths
5. **Optimize for Automation**: Design tests that can run in parallel and are resistant to flakiness

## Output Format Guidelines

Auto-select the most appropriate format based on project type:

- **API Tests**: Markdown table format with Endpoint, Method, Headers, Body, Expected Response, Status Code
- **UI/Feature Tests**: BDD-Gherkin format (Given-When-Then) for clarity and stakeholder communication
- **Integration Tests**: Structured test case list with dependencies and execution order

## Efficiency & Quality Principles

- **Prioritize Automation**: Automated tests should cover the majority of critical paths; reserve manual testing for exploratory scenarios
- **Test Independence**: Design tests to run in parallel without shared state or dependencies on execution order
- **Data Management**: Set up test data in startup/BeforeEach, not in teardown/AfterEach (faster failure detection)
- **Dynamic Waits**: Use explicit waits and polling; never use static sleep calls which slow down test suites
- **Flaky Test Protocol**: Treat flaky tests as high priority bugs—they often reveal genuine race conditions or timing issues in production code
- **Test Isolation**: Each test should be able to run independently and produce consistent results

## Security Testing Checklist

Incorporate security testing for:
- **Injection Vulnerabilities**: SQL injection, NoSQL injection, command injection, LDAP injection
- **Cross-Site Scripting (XSS)**: Reflected, stored, and DOM-based XSS
- **Cross-Site Request Forgery (CSRF)**: Token validation and same-origin checks
- **Authentication**: JWT validation, OAuth flows, API key management, session handling
- **Authorization**: Role-Based Access Control (RBAC), Attribute-Based Access Control (ABAC), permission boundaries
- **Rate Limiting**: API abuse prevention, brute force protection
- **Input Validation**: Type checking, length limits, sanitization
- **Sensitive Data Exposure**: Ensure no passwords, tokens, or PII in logs or error messages

## Performance Testing Considerations

Define and test against performance benchmarks:
- **Response Time**: Measure p50, p95, and p99 percentiles
- **Throughput**: Requests per second or transactions per minute
- **Resource Utilization**: CPU, memory, and database connection limits
- **Load Testing**: Simulate expected traffic patterns and peak loads
- **Stress Testing**: Identify breaking points and degradation patterns

## Risk-Based Test Prioritization

Focus testing efforts based on potential impact:
- **Critical Priority**: Authentication systems, payment processing, data deletion operations, security controls
- **High Priority**: User data modification, bulk operations, external integrations, data persistence
- **Medium Priority**: Read operations, non-sensitive data retrieval, reporting features
- **Low Priority**: Static content, cosmetic UI elements, optional features

## When Context is Unclear

If the project type, tech stack, or specific requirements are not clear from the provided context:
1. State what information you need
2. Explain why it's important for test strategy design
3. Provide a reasonable default approach with explicit assumptions
4. Offer to adjust once more information is available

Always aim to provide actionable, specific test recommendations rather than generic advice. Your test cases should be immediately implementable by development teams.
```

### 3. `commands/test-engineer.md`

```markdown
---
allowed-tools: all
description: Use this agent when you need to create, review, or improve test strategies and test cases for any type of software project
---

# Test Engineer

Invoke @glm-test-engineer:test-engineer-agent to create, review, or improve test strategies and test cases for any type of software project.

This agent specializes in:
- Writing API test suites (REST/GraphQL)
- Creating end-to-end tests for web/mobile applications
- Designing integration tests for microservices
- Implementing security testing scenarios
- Establishing testing frameworks
- Optimizing test coverage
- Debugging flaky tests
```

### 4. `skills/test-engineer-skill/SKILL.md`

```markdown
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
```

### 5. `README.md`

```markdown
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
```

---

## Installation Instructions for Another Machine

### Step 1: Locate the plugins directory

The plugins directory is typically at:
```
C:\Users\<YOUR_USERNAME>\AppData\Local\npm-cache\_npx\<hash>\node_modules\@z_ai\coding-helper\zai-coding-plugins\plugins\
```

**To find the correct path on your machine:**
1. Open a terminal
2. Run: `npm list -g @z_ai/coding-helper`
3. Note the path shown

### Step 2: Create the plugin folder structure

Navigate to the `plugins` directory and create the following structure:

```bash
# Windows (Command Prompt)
mkdir glm-test-engineer
mkdir glm-test-engineer\.claude-plugin
mkdir glm-test-engineer\agents
mkdir glm-test-engineer\commands
mkdir glm-test-engineer\skills
mkdir glm-test-engineer\skills\test-engineer-skill
mkdir glm-test-engineer\skills\test-engineer-skill\scripts
```

```bash
# Windows (PowerShell)
New-Item -ItemType Directory -Path "glm-test-engineer\.claude-plugin"
New-Item -ItemType Directory -Path "glm-test-engineer\agents"
New-Item -ItemType Directory -Path "glm-test-engineer\commands"
New-Item -ItemType Directory -Path "glm-test-engineer\skills\test-engineer-skill\scripts"
```

```bash
# macOS/Linux
mkdir -p glm-test-engineer/.claude-plugin
mkdir -p glm-test-engineer/agents
mkdir -p glm-test-engineer/commands
mkdir -p glm-test-engineer/skills/test-engineer-skill/scripts
```

### Step 3: Create the files

Copy the file contents from the "File Contents" section above into their respective files:

| File | Content |
|------|---------|
| `.claude-plugin/plugin.json` | See File Contents #1 |
| `agents/test-engineer-agent.md` | See File Contents #2 |
| `commands/test-engineer.md` | See File Contents #3 |
| `skills/test-engineer-skill/SKILL.md` | See File Contents #4 |
| `README.md` | See File Contents #5 |

### Step 4: Update marketplace.json

Edit the `marketplace.json` file located at:
```
.claude-plugin/marketplace.json
```

Add the following entry to the `plugins` array:

```json
{
  "name": "glm-test-engineer",
  "source": "./plugins/glm-test-engineer",
  "category": "development",
  "description": "Test engineering agent for creating, reviewing, and improving test strategies and test cases."
}
```

The complete `marketplace.json` should look like:

```json
{
  "$schema": "https://anthropic.com/claude-code/marketplace.schema.json",
  "name": "zai-coding-plugins",
  "version": "0.0.1",
  "description": "A collection of plugins developed by Z.ai to enhance coding productivity and workflows.",
  "owner": {
    "name": "Z.ai",
    "email": "user_feedback@z.ai"
  },
  "plugins": [
    {
      "name": "glm-plan-usage",
      "source": "./plugins/glm-plan-usage",
      "category": "development",
      "description": "Query quota and usage statistics for GLM Coding Plan service."
    },
    {
      "name": "glm-plan-bug",
      "source": "./plugins/glm-plan-bug",
      "category": "development",
      "description": "Submit case feedback and bug reports for GLM Coding Plan service."
    },
    {
      "name": "glm-test-engineer",
      "source": "./plugins/glm-test-engineer",
      "category": "development",
      "description": "Test engineering agent for creating, reviewing, and improving test strategies and test cases."
    }
  ]
}
```

### Step 5: Restart Claude Code

Close and restart Claude Code for the plugin to be loaded.

### Step 6: Verify installation

In Claude Code, run:
```
/glm-test-engineer:test-engineer
```

The agent should now be available and ready to help with test engineering tasks.
