# Lens 4: Quality & Testing Excellence

## Overview

The Quality & Testing lens provides a comprehensive framework for implementing agentic testing strategies across functional, visual, accessibility, performance, security, and resilience testing. This module transforms testing from a manual, post-development activity into an automated, specification-driven process that runs throughout the development lifecycle.

## Core Technology Stack

| Technology | Purpose | Application |
|-----------|---------|-------------|
| **Figma MCP** | Design-to-test automation | Generate tests from visual specifications |
| **BDD (Behavior-Driven Development)** | Methodology | Gherkin scenarios, stakeholder collaboration |
| **TDD (Test-Driven Development)** | Methodology | Red-green-refactor cycle |
| **Selenium MCP** | Browser automation | Natural language UI testing |
| **Playwright MCP** | Modern browser testing | Auto-waiting, network mocking, trace files |
| **Claude Agentic SDK** | AI orchestration | Test generation, analysis |
| **LangChain** | Agent framework | Multi-agent test orchestration |
| **AWS FIS (Fault Injection)** | Chaos engineering | Resilience testing automation |

## Table of Contents

1. [Introduction: The Agentic Testing Paradigm](#1-introduction-the-agentic-testing-paradigm)
2. [SDD → BDD → TDD Hierarchy](#2-sdd--bdd--tdd-hierarchy)
3. [Browser MCP: Selenium & Playwright](#3-browser-mcp-selenium--playwright)
4. [Design-to-Test Automation with Figma MCP](#4-design-to-test-automation-with-figma-mcp)
5. [Multi-Faceted Testing Strategy](#5-multi-faceted-testing-strategy)
6. [Performance & Load Testing](#6-performance--load-testing)
7. [Security & Vulnerability Testing](#7-security--vulnerability-testing)
8. [Agentic Chaos Engineering](#8-agentic-chaos-engineering)
9. [Best Practices & Patterns](#9-best-practices--patterns)

---

## 1. Introduction: The Agentic Testing Paradigm

### The Paradigm Shift

Traditional testing follows a reactive model:

```text
Spec → Code → Write Tests (often skipped due to time pressure)
```

Agentic testing inverts this:

```text
Spec → Auto-Generate Tests → Code to Pass Tests
```

**Result:** 100% test coverage from day one, tests serve as the specification.

### Why This Matters for FSI

Financial services institutions face unique testing challenges:

- **Regulatory Compliance**: SOC 2, PCI DSS, GDPR require comprehensive testing evidence
- **Zero-Error Tolerance**: Financial transactions must be 100% accurate
- **High Availability**: 99.9%+ uptime SLAs
- **Security Critical**: Constant threat landscape
- **Audit Requirements**: Complete traceability from requirements to tests to code

Agentic testing provides:

- ✅ Automated compliance evidence
- ✅ Comprehensive test coverage
- ✅ Continuous validation
- ✅ Complete audit trail
- ✅ Faster time-to-market

---

## 2. SDD → BDD → TDD Hierarchy

### The Three-Layer Testing Foundation

```text
┌──────────────────────────────────────────────────────────┐
│  Layer 1: SDD (Spec-Driven Development)                  │
│  Purpose: Define WHAT to build                           │
│  Artifact: requirements.md, design.md, spec.md           │
│  Stakeholder: Product Manager, Business Analyst          │
└─────────────────┬────────────────────────────────────────┘
                  │
                  ▼
┌──────────────────────────────────────────────────────────┐
│  Layer 2: BDD (Behavior-Driven Development)              │
│  Purpose: Define HOW it should behave                    │
│  Artifact: feature.spec (Gherkin)                        │
│  Stakeholder: QA Engineer, Product Manager, Developer    │
└─────────────────┬────────────────────────────────────────┘
                  │
                  ▼
┌──────────────────────────────────────────────────────────┐
│  Layer 3: TDD (Test-Driven Development)                  │
│  Purpose: Define implementation correctness              │
│  Artifact: unit.test.ts, integration.test.ts             │
│  Stakeholder: Developer                                  │
└──────────────────────────────────────────────────────────┘
```

### Gherkin: The Ubiquitous Language

**Gherkin** is the critical bridge between human-readable requirements and machine-executable tests. It provides a structured, stakeholder-accessible format that translates directly into automated test code.

**Gherkin Structure:**

```gherkin
Feature: [High-level capability]
  As a [role]
  I want to [action]
  So that [business value]

  Background:
    Given [precondition for all scenarios]

  Scenario: [Specific test case]
    Given [initial context]
    When [action]
    Then [expected outcome]
    And [additional expectation]

  Scenario Outline: [Parameterized test case]
    Given [context with <parameter>]
    When [action with <parameter>]
    Then [outcome with <parameter>]

    Examples:
      | parameter1 | parameter2 | expected |
      | value1     | value2     | result1  |
```

#### Example: Banking Transfer Feature

```gherkin
Feature: Inter-Account Fund Transfer
  As a banking customer
  I want to transfer funds between my accounts
  So that I can manage my finances efficiently

  Background:
    Given I am logged in to my account
    And I have a checking account with balance $5000
    And I have a savings account with balance $10000
    And my daily transfer limit is $2000

  Scenario: Successful transfer within limits
    When I transfer $500 from checking to savings
    Then the transfer should be completed
    And my checking balance should be $4500
    And my savings balance should be $10500
    And I should receive a confirmation notification

  Scenario: Transfer exceeds available balance
    When I transfer $6000 from checking to savings
    Then the transfer should be rejected
    And I should see error "Insufficient funds"
    And no account balances should change

  Scenario: Transfer exceeds daily limit
    Given I have already transferred $1800 today
    When I transfer $500 from checking to savings
    Then the transfer should be rejected
    And I should see error "Daily transfer limit exceeded"
    And I should see "Remaining limit: $200"

  Scenario Outline: Multiple transfer amounts
    When I transfer $<amount> from checking to savings
    Then the transfer should be <status>
    And my checking balance should be $<checking_final>

    Examples:
      | amount | status    | checking_final |
      | 100    | completed | 4900          |
      | 500    | completed | 4500          |
      | 5000   | rejected  | 5000          |
      | 0      | rejected  | 5000          |
```

### From Gherkin to Executable Tests

```typescript
// Auto-generated from Gherkin using Cucumber.js
import { Given, When, Then } from '@cucumber/cucumber';
import { expect } from '@playwright/test';

Given('I am logged in to my account', async function() {
  await this.page.goto('/login');
  await this.page.fill('[data-testid="email"]', 'user@example.com');
  await this.page.fill('[data-testid="password"]', 'SecurePass123!');
  await this.page.click('[data-testid="sign-in-button"]');
  await expect(this.page).toHaveURL('/dashboard');
});

Given('I have a checking account with balance ${int}', async function(balance) {
  // Seed test data via API
  await this.api.post('/test/seed-account', {
    accountType: 'checking',
    balance: balance
  });
});

When('I transfer ${int} from checking to savings', async function(amount) {
  await this.page.click('[data-testid="transfer-button"]');
  await this.page.selectOption('[data-testid="from-account"]', 'checking');
  await this.page.selectOption('[data-testid="to-account"]', 'savings');
  await this.page.fill('[data-testid="amount"]', amount.toString());
  await this.page.click('[data-testid="submit-transfer"]');
});

Then('the transfer should be completed', async function() {
  await expect(this.page.locator('[data-testid="success-message"]'))
    .toContainText('Transfer completed');
});

Then('my checking balance should be ${int}', async function(expectedBalance) {
  const balance = await this.page.locator('[data-testid="checking-balance"]')
    .textContent();
  expect(parseFloat(balance.replace('$', ''))).toBe(expectedBalance);
});
```

---

## 3. Browser MCP: Selenium & Playwright

### The Testing MCP Revolution

Browser MCP servers enable **natural language testing**—QA engineers and even product managers can issue test commands without writing code.

### Selenium MCP

**Traditional Selenium:**

```java
// Complex, brittle code
WebDriver driver = new ChromeDriver();
driver.get("https://example.com/login");
WebElement emailField = driver.findElement(By.id("email"));
emailField.sendKeys("user@example.com");
WebElement passwordField = driver.findElement(By.id("password"));
passwordField.sendKeys("password123");
WebElement loginButton = driver.findElement(By.xpath("//button[@type='submit']"));
loginButton.click();
WebDriverWait wait = new WebDriverWait(driver, 10);
wait.until(ExpectedConditions.urlContains("/dashboard"));
```

**Agentic Approach with Selenium MCP:**

```text
QA Engineer or Product Manager (in natural language):

"Navigate to the login page, enter 'user@example.com' in the email field, 
enter 'password123' in the password field, and click the login button"

↓ Selenium MCP Server translates ↓

Executable Selenium commands → Test execution
```

**Key Innovation:**
Instead of writing complex test scripts, a QA engineer or product manager can issue **natural language commands**. The Selenium MCP server translates these commands into executable Selenium actions.

### Playwright MCP: Modern Alternative

**Advantages over Selenium:**

- ✅ Built-in auto-waiting (no explicit waits needed)
- ✅ Better handling of modern web apps (SPAs, PWAs)
- ✅ Network interception and mocking
- ✅ Multiple browser contexts
- ✅ Video recording and trace files
- ✅ Better performance
- ✅ Parallel execution
- ✅ Auto-retry on flaky tests

**Example with Playwright MCP:**

```typescript
// Natural language → Playwright MCP → Executable test
Agent: "Test the login flow with network offline simulation"

Generated Playwright code:
test('Login with offline network', async ({ page, context }) => {
  // Simulate offline
  await context.setOffline(true);
  
  await page.goto('/login');
  
  // Expect offline message
  await expect(page.locator('.offline-message')).toBeVisible();
  
  // Restore network
  await context.setOffline(false);
  
  // Login should now work
  await page.fill('[data-testid="email"]', 'user@example.com');
  await page.fill('[data-testid="password"]', 'password123');
  await page.click('[data-testid="sign-in-button"]');
  
  await expect(page).toHaveURL('/dashboard');
});
```

### Enabled QA Workflows

#### 1. AI-Powered Test Execution

```text
Agent Prompt:
"Test the checkout flow:
1. Click the 'Add to Cart' button on the first product
2. Click the cart icon in the header
3. Click 'Proceed to Checkout'
4. Fill out the shipping form with test data
5. Select 'Credit Card' as payment method
6. Verify the order summary shows correct totals
7. Click 'Place Order'"

Agent executes via Selenium/Playwright MCP → Full test run with screenshots
```

#### 2. Faster Debugging & Root Cause Analysis

```text
Test Failure:
"Login test failed at step 3: Click login button"

Agent with Browser MCP access:
- Retrieves browser console logs
- Captures network requests
- Takes screenshot at failure point
- Analyzes DOM state
- Provides RCA: "Login button was disabled due to email validation error"
```

#### 3. AI-Assisted Test Case Generation

```text
Agent Prompt:
"Analyze the last 10 login test executions and generate 5 new edge case 
test scenarios that weren't covered"

Agent Output (via Browser MCP analysis):
1. Test login with email containing special characters (+, .)
2. Test login with maximum password length (128 characters)
3. Test login during session timeout
4. Test login with caps lock warning
5. Test login with autofill credentials
```

---

## 4. Design-to-Test Automation with Figma MCP

### Figma as the Origin Point of Testable UI

The Figma MCP server's **true architectural significance** is not just in code generation, but in its role as the **origin point for automated, design-first testing**.

**Key Insight:**
The server provides **programmatic access** to all:

- UI elements
- Interactions
- Flow information
- Layout data

This structured data is **precisely what is needed to generate test cases**.

### Revolutionary Workflow

```text
BDD scenarios are generated from the design
BEFORE implementation code has been written
```

### Step-by-Step: From Figma to Automated Tests

#### Step 1: Design Contains Flow Information

#### Example: User Login Flow in Figma

```text
Frame: "Login Screen"
  ├─ Input: "Email" (required, type=email)
  ├─ Input: "Password" (required, type=password)
  ├─ Checkbox: "Remember me"
  ├─ Button: "Sign In" (onClick → "Dashboard Screen")
  └─ Link: "Forgot password?" (onClick → "Password Reset Screen")

Frame: "Dashboard Screen"
  ├─ Header: "Welcome, {user.name}"
  └─ ...
```

#### Step 2: Agent Extracts Flow via MCP

```python
# Pseudo-code for test generation agent
agent_prompt = """
Extract all user flows from the Figma file and generate BDD test scenarios.
For each interactive element, identify:
- User action (click, type, select)
- Expected outcome (navigation, state change, validation)
- Error states
"""

figma_data = mcp_client.query(
    tool="extract_design_context",
    file_url="https://figma.com/file/banking-app"
)

flows = extract_flows(figma_data)
```

#### Step 3: Generate Gherkin Test Scenarios

**Auto-Generated BDD Test:**

```gherkin
Feature: User Authentication
  As a banking customer
  I want to securely log in to my account
  So that I can access my financial information

  Background:
    Given I am on the login page

  Scenario: Successful login with valid credentials
    When I enter "user@example.com" in the "Email" field
    And I enter "SecurePass123!" in the "Password" field
    And I click the "Sign In" button
    Then I should be redirected to the "Dashboard" page
    And I should see "Welcome, John Doe"

  Scenario: Failed login with invalid email format
    When I enter "invalid-email" in the "Email" field
    And I click the "Sign In" button
    Then I should see an error message "Please enter a valid email address"
    And I should remain on the login page

  Scenario: Failed login with wrong password
    When I enter "user@example.com" in the "Email" field
    And I enter "WrongPassword" in the "Password" field
    And I click the "Sign In" button
    Then I should see an error message "Invalid email or password"
    And the "Password" field should be cleared
    And I should remain on the login page

  Scenario: Password reset flow
    When I click the "Forgot password?" link
    Then I should be redirected to the "Password Reset" page

  Scenario: Remember me functionality
    When I enter valid credentials
    And I check the "Remember me" checkbox
    And I click the "Sign In" button
    And I log out
    And I return to the login page
    Then the "Email" field should be pre-filled with "user@example.com"
```

#### Step 4: Generate Executable Test Code

```typescript
// Auto-generated from BDD scenarios using Playwright
import { test, expect } from '@playwright/test';

test.describe('User Authentication', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('Successful login with valid credentials', async ({ page }) => {
    await page.fill('[data-testid="email-input"]', 'user@example.com');
    await page.fill('[data-testid="password-input"]', 'SecurePass123!');
    await page.click('[data-testid="sign-in-button"]');
    
    await expect(page).toHaveURL('/dashboard');
    await expect(page.locator('[data-testid="welcome-message"]'))
      .toContainText('Welcome, John Doe');
  });

  test('Failed login with invalid email format', async ({ page }) => {
    await page.fill('[data-testid="email-input"]', 'invalid-email');
    await page.click('[data-testid="sign-in-button"]');
    
    await expect(page.locator('[data-testid="email-error"]'))
      .toContainText('Please enter a valid email address');
    await expect(page).toHaveURL('/login');
  });
});
```

### Benefits of Design-First Testing

#### 1. Tests Written Before Code

**Traditional Workflow:**

```text
Design → Code → Write Tests (often skipped due to time pressure)
```

**Agentic Workflow:**

```text
Design → Auto-Generate Tests → Code to Pass Tests
```

**Result:** 100% test coverage from day one, tests serve as the specification.

#### 2. Design as Living Documentation

The Figma file becomes the **single source of truth**:

- Product requirements
- Visual specification
- Test scenarios
- Implementation checklist

**Version Control Integration:**

```text
Each Figma version → New test suite generated
Developers can see exactly what changed and what tests need updating
```

#### 3. Compliance and Audit Trail

For FSI organizations:

**Audit Question:** "How do you ensure your login flow meets security requirements?"

**Answer with Evidence:**

```text
1. Security requirements documented in Figma (annotations)
2. Test scenarios auto-generated from design (Gherkin files in git)
3. Test execution reports (Playwright HTML reports)
4. Implementation code (React components in git)

Complete trail: Requirement → Design → Test → Code
```

---

## 5. Multi-Faceted Testing Strategy

### 1. Functional Verification

#### Agentic TDD (Test-Driven Development)

A developer can guide an agent through the **"red-green-refactor" TDD loop**. The developer provides **critical architectural decisions** and uses guardrails like pre-commit scripts to keep the agent on track.

**The TDD Loop with AI Agents:**

```text
Step 1: RED (Write Failing Test)
──────────────────────────────────
Developer: "Write a unit test for the calculateMonthlyPayment function 
            that validates loan amortization calculations"

Agent generates:
describe('calculateMonthlyPayment', () => {
  it('should calculate correct monthly payment for 30-year mortgage', () => {
    const result = calculateMonthlyPayment({
      principal: 300000,
      annualRate: 0.04,
      years: 30
    });
    expect(result).toBeCloseTo(1432.25, 2);
  });
});

Run test → ❌ FAILS (function doesn't exist)

Step 2: GREEN (Write Minimal Code to Pass)
──────────────────────────────────────────
Developer: "Implement the calculateMonthlyPayment function to pass the test"

Agent implements:
function calculateMonthlyPayment({ principal, annualRate, years }) {
  const monthlyRate = annualRate / 12;
  const numPayments = years * 12;
  const payment = principal * 
    (monthlyRate * Math.pow(1 + monthlyRate, numPayments)) / 
    (Math.pow(1 + monthlyRate, numPayments) - 1);
  return payment;
}

Run test → ✅ PASSES

Step 3: REFACTOR (Improve Code Quality)
───────────────────────────────────────
Developer: "Refactor for readability and add edge case handling"

Agent refactors with TypeScript, validation, edge cases
Run test → ✅ PASSES (with better code)
```

#### Agentic BDD (Behavior-Driven Development)

Frameworks like **BDDTestAIGen** use LLMs to automate the creation of BDD tests directly from high-level natural language requirements.

**Traditional BDD Challenge:**

```text
Problem: Writing comprehensive Gherkin scenarios requires:
- Deep understanding of the application
- Knowledge of edge cases
- Consistent language and structure
- Significant time investment

Result: Incomplete test coverage, inconsistent scenarios
```

**BDDTestAIGen Solution:**

```text
Input (Natural Language Requirement):
"Users should be able to transfer funds between their checking and savings 
accounts. The transfer should validate sufficient balance, apply daily 
transfer limits, and send confirmation notifications."

↓ BDDTestAIGen Agent ↓

Output: Complete Gherkin scenarios with edge cases, error conditions,
        happy paths, and boundary conditions
```

**Key Benefits:**

- ✅ Comprehensive Coverage - Agent generates scenarios you might miss
- ✅ Consistent Structure - All scenarios follow the same pattern
- ✅ Edge Case Discovery - Agent thinks of boundary conditions
- ✅ Fast Iteration - Generate scenarios in seconds, not hours
- ✅ Stakeholder Accessibility - Non-technical team members can contribute

### 2. Visual Validation: Beyond Pixel-Perfect Testing

**Traditional Visual Testing:**

```text
❌ Problem: Pixel-perfect snapshot comparison

Test fails if:
- Font rendering differs slightly between OS
- Browser renders 1px differently
- Dynamic content changes (dates, user names)
- Loading states differ in timing
- Minor CSS tweaks (1px padding change)

Result: 50%+ false positive rate → Developers ignore visual tests
```

**AI-Powered Visual Validation:**

AI-native testing platforms like **Mabl** employ AI for **semantic understanding of UI components**.

```text
✅ Solution: Semantic understanding of UI components

Mabl AI validates:
- Component structure (button is still a button)
- Text content (ignoring minor formatting)
- Layout relationships (button is still below input)
- Visual hierarchy (headings are still prominent)
- Functional elements (clickable areas intact)

Ignores:
- Minor pixel shifts
- Font rendering differences
- Dynamic content (timestamps, user-specific data)
- Intentional design updates

Result: <5% false positive rate → Tests are trusted
```

**Example:**

```typescript
// AI-powered visual validation (robust)
test('Login form maintains structure and functionality', async ({ page }) => {
  await page.goto('/login');
  
  await mabl.visualValidation(page, {
    validateComponents: [
      { type: 'input', label: 'Email', required: true },
      { type: 'input', label: 'Password', required: true },
      { type: 'button', text: 'Sign In', primary: true },
      { type: 'link', text: 'Forgot password?' }
    ],
    validateLayout: {
      emailAbovePassword: true,
      buttonBelowInputs: true,
      forgotPasswordRightAligned: true
    },
    ignoreContent: ['timestamp', 'username'],
    tolerance: 'semantic'
  });
});
```

### 3. Accessibility Testing: Automated WCAG Compliance

Accessibility is **no longer a separate, manual audit**. Modern AI test tools integrate **automated WCAG-based validation** directly into the test execution flow.

**The Problem with Traditional A11y Testing:**

```text
❌ Manual audits (expensive, infrequent)
❌ Separate test suites (out of sync with main tests)
❌ Requires specialized expertise
❌ Often tested at the end (too late to fix easily)
❌ Incomplete coverage
```

**Agentic Accessibility Testing:**

```typescript
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test.describe('Login Page Accessibility', () => {
  test('should not have any WCAG violations', async ({ page }) => {
    await page.goto('/login');
    
    const accessibilityScanResults = await new AxeBuilder({ page })
      .withTags(['wcag2a', 'wcag2aa', 'wcag21a', 'wcag21aa'])
      .analyze();
    
    expect(accessibilityScanResults.violations).toEqual([]);
  });
  
  test('should support full keyboard navigation', async ({ page }) => {
    await page.goto('/login');
    
    await page.keyboard.press('Tab'); // Email input
    await expect(page.locator('[data-testid="email-input"]')).toBeFocused();
    
    await page.keyboard.press('Tab'); // Password input
    await expect(page.locator('[data-testid="password-input"]')).toBeFocused();
    
    await page.keyboard.press('Tab'); // Sign in button
    await expect(page.locator('[data-testid="sign-in-button"]')).toBeFocused();
    
    await page.keyboard.press('Enter'); // Submit
  });
  
  test('should have proper ARIA labels', async ({ page }) => {
    await page.goto('/login');
    
    const emailInput = page.locator('input[type="email"]');
    await expect(emailInput).toHaveAttribute('aria-label', /email/i);
    
    const passwordInput = page.locator('input[type="password"]');
    await expect(passwordInput).toHaveAttribute('aria-label', /password/i);
  });
});
```

**FSI Compliance Benefits:**

```text
Banking Application A11y Requirements:
- ADA compliance (Americans with Disabilities Act)
- WCAG 2.1 AA minimum (often AAA for critical flows)
- Section 508 compliance (government banking)

Agentic Testing ensures:
✅ Automated validation on every commit
✅ Compliance evidence for audits
✅ Reduced legal risk
✅ Better user experience for all customers
```

---

## 6. Performance & Load Testing

```gherkin
Feature: Transaction API Performance
  As a system architect
  I want to ensure the transaction API meets performance SLAs
  So that users experience fast, reliable service

  Scenario: Handle baseline load
    Given the system is in a healthy state
    When 100 concurrent users request transaction history
    Then 95th percentile response time should be under 200ms
    And 99th percentile response time should be under 500ms
    And error rate should be 0%

  Scenario: Handle peak load
    Given the system is in a healthy state
    When 1000 concurrent users request transaction history for 5 minutes
    Then 95th percentile response time should be under 1000ms
    And error rate should be under 0.1%

  Scenario: Graceful degradation under extreme load
    When 5000 concurrent users request transaction history
    Then the system should return 429 (Too Many Requests) for excess traffic
    And existing users should maintain sub-1s response times
    And no data corruption should occur
```

**Agent Implementation with k6:**

```typescript
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate } from 'k6/metrics';

const errorRate = new Rate('errors');

export const options = {
  stages: [
    { duration: '2m', target: 100 },  // Baseline
    { duration: '5m', target: 1000 }, // Peak
    { duration: '2m', target: 5000 }, // Extreme
    { duration: '2m', target: 0 },    // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<1000', 'p(99)<2000'],
    errors: ['rate<0.001'],
  },
};

export default function () {
  const response = http.get('https://api.bank.com/transactions', {
    headers: { 'Authorization': 'Bearer ${TOKEN}' },
  });
  
  check(response, {
    'status is 200': (r) => r.status === 200,
    'response time < 1s': (r) => r.timings.duration < 1000,
  }) || errorRate.add(1);
  
  sleep(1);
}
```

---

## 7. Security & Vulnerability Testing

```gherkin
Feature: API Security
  As a security engineer
  I want to ensure the API is protected against common vulnerabilities
  So that customer data remains secure

  Scenario: Prevent SQL injection
    When I send a request with SQL injection payload in the email parameter
    Then the request should be rejected with 400 Bad Request
    And no database queries should be executed
    And the incident should be logged

  Scenario: Enforce rate limiting
    When I send 100 requests in 10 seconds
    Then requests 51-100 should return 429 Too Many Requests
    And a rate limit header should indicate reset time

  Scenario: Validate JWT token expiration
    Given I have an expired JWT token
    When I send a request with the expired token
    Then the request should return 401 Unauthorized
    And the response should indicate "Token expired"
```

---

## 8. Agentic Chaos Engineering

### From Manual Gamedays to Continuous Resilience Testing

**Traditional Chaos Engineering:**

```text
❌ Manual execution (quarterly "gamedays")
❌ Requires dedicated team
❌ Disruptive to development workflow
❌ Limited coverage
❌ Results hard to track over time
```

**Agentic Chaos Engineering:**

```text
✅ Automated, continuous execution
✅ Integrated into CI/CD pipeline
✅ Non-disruptive (runs in isolated environments)
✅ Comprehensive coverage
✅ Tracked as code in version control
```

### BDD for Resilience Testing

```gherkin
Feature: EKS Pod Resilience
  As a platform engineer
  I want to ensure our application survives pod failures
  So that we meet our 99.9% uptime SLA

  Background:
    Given the production environment is healthy
    And all CloudWatch alarms are in OK state
    And the application has at least 10 running pods

  Scenario: Survive partial pod failure
    When the agent executes an AWS FIS experiment to "terminate 10% of EKS pods"
    Then the system availability should remain above 99.9%
    And the CloudWatch "PodCountLow" alarm should fire within 1 minute
    And Kubernetes should auto-heal within 2 minutes
    And no customer-facing errors should be logged
    And API response times should remain under 500ms

  Scenario: Survive availability zone failure
    When the agent executes an AWS FIS experiment to "make us-east-1a unavailable"
    Then traffic should route to us-east-1b and us-east-1c
    And system availability should remain above 99.5%
    And all active user sessions should be preserved

  Scenario: Survive database primary failover
    When the agent executes an AWS FIS experiment to "force RDS failover"
    Then the application should detect failover within 10 seconds
    And reconnect to new primary within 15 seconds
    And in-flight transactions should be retried
    And no data loss should occur
```

### Agent Implementation

```typescript
import { test, expect } from '@playwright/test';
import { FISClient, StartExperimentCommand } from '@aws-sdk/client-fis';
import { CloudWatchClient } from '@aws-sdk/client-cloudwatch';

test('Survive partial pod failure', async () => {
  const fisClient = new FISClient({ region: 'us-east-1' });
  
  // Verify baseline
  const baseline = await checkSystemHealth();
  expect(baseline.availability).toBeGreaterThan(99.9);
  
  // Execute chaos experiment
  const experiment = await fisClient.send(new StartExperimentCommand({
    experimentTemplateId: 'EXP-Pod-Termination-10pct',
    tags: { 'Test': 'Automated' }
  }));
  
  // Monitor during chaos
  const monitoring = monitorSystem(experiment.experiment.id, {
    duration: 300,
    metrics: ['availability', 'error_rate', 'response_time']
  });
  
  // Validate availability maintained
  const availability = await monitoring.getMetric('availability');
  expect(availability.min).toBeGreaterThan(99.9);
  
  // Validate no customer impact
  const errors = await queryErrorLogs(experiment.timeRange);
  expect(errors.customerFacing).toBe(0);
});
```

### CI/CD Integration

```yaml
# .github/workflows/chaos-testing.yml
name: Continuous Chaos Engineering

on:
  schedule:
    - cron: '0 2 * * *' # Daily at 2 AM

jobs:
  chaos-tests:
    runs-on: ubuntu-latest
    environment: staging
    
    steps:
      - uses: actions/checkout@v3
      - name: Run Chaos Tests
        run: npx playwright test chaos/*.spec.ts
      - name: Upload Report
        uses: actions/upload-artifact@v3
        with:
          name: chaos-test-results
          path: playwright-report/
```

---

## 9. Best Practices & Patterns

### Test Organization

**Directory Structure:**

```text
tests/
├── functional/
│   ├── login.spec.ts
│   ├── transfer.spec.ts
│   └── dashboard.spec.ts
├── visual/
│   ├── components.visual.ts
│   └── pages.visual.ts
├── accessibility/
│   ├── wcag2aa.spec.ts
│   └── keyboard-nav.spec.ts
├── performance/
│   ├── load-test.k6.js
│   └── stress-test.k6.js
├── security/
│   ├── injection.spec.ts
│   └── auth.spec.ts
└── chaos/
    ├── pod-failure.spec.ts
    └── network-latency.spec.ts
```

### Test Data Management

**Use APIs for test data:**

```typescript
test.beforeEach(async ({ request }) => {
  await request.post('/api/test/seed', {
    data: {
      user: { email: 'test@example.com', balance: 5000 },
      accounts: [
        { type: 'checking', balance: 5000 },
        { type: 'savings', balance: 10000 }
      ]
    }
  });
});

test.afterEach(async ({ request }) => {
  await request.delete('/api/test/cleanup');
});
```

### Continuous Testing Strategy

```text
Commit → Unit Tests (30s)
   ↓
Push → Integration Tests (5min)
   ↓
PR → Visual Tests (10min)
   ↓
Merge → E2E Tests (20min)
   ↓
Deploy → Smoke Tests (5min)
   ↓
Production → Continuous Monitoring
```

---

## Related Modules

- **[Lens 1: Product Development & Architecture](../lens-1-product-dev-architecture/README.md)** - Design and development workflows
- **[Lens 3: Platform, Cloud & SRE](../lens-3-platform-cloud-sre/README.md)** - Infrastructure reliability
- **[Lens 5: AI, Inference & Agentic Automation](../lens-5-ai-inference-automation/README.md)** - Advanced AI orchestration

---

## Additional Resources

- **[Complete Workflow Guide](../../AGENTIC_WORKFLOW_PRACTICE.md)** - Full platform documentation
- **[Banking Use Case](../../AGENTIC_BANK_FSI.md)** - Financial services implementation
- **[Main README](../../README.md)** - Platform overview

---

**Status**: Full content migration complete

**Last Updated**: 2025-01-17

**Maintainers**: QA & Platform Architecture Team
