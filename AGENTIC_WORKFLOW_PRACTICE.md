# The Agentic Workflow in Practice: Design, Test, and Execution

## A Complete Guide to Design-First, Spec-Driven Development with AI Agents

---

## Table of Contents

- [Overview](#overview)
- [The Design Phase: Figma MCP](#the-design-phase-figma-mcp)
- [The Testing Phase: Automated Test Generation](#the-testing-phase-automated-test-generation)
- [The Execution Phase: Agentic Code Generation](#the-execution-phase-agentic-code-generation)
- [Complete Workflow Example](#complete-workflow-example)
- [Best Practices](#best-practices)
- [Integration with SDD Methodology](#integration-with-sdd-methodology)
- [Tools and Resources](#tools-and-resources)

---

## Overview

This guide details the practical, day-to-day Software Development Lifecycle (SDLC) for agentic AI development, progressing through three critical phases:

1. **Design** - Visual specification using Figma MCP
2. **Test** - Automated test generation from design intent
3. **Execution** - AI agents writing production code

The workflow represents a fundamental shift from traditional development: **specifications begin as visual designs**, tests are **generated automatically from those designs**, and code is **written by AI agents** to satisfy both the design intent and the generated tests.

### Why This Matters for Financial Services

For FSI organizations, this workflow provides:

- **Compliance by Design** - Audit trails from visual spec → tests → code
- **Rapid Prototyping** - From design to working prototype in hours, not weeks
- **Design System Consistency** - Automated enforcement of UI/UX standards
- **Reduced Technical Debt** - Generated code follows established patterns
- **Accelerated Time-to-Market** - 10x faster development cycles

---

## The Design Phase: Figma MCP

### Introduction: The 'Design MCP' as the Bridge

The SDD (Spec-Driven Development) workflow begins with the **specification**, which for modern applications is increasingly **visual**. The **Figma MCP (Model Context Protocol) server** is the purpose-built tool that exposes Figma design files, FigJam diagrams, and Make files as **structured context** to AI agents.

**Key Insight:**  
Figma MCP functions as the **critical architectural bridge**, translating the visual specification into a format that a code-generation agent can understand and implement.

---

### Connection and Access

An MCP client (such as an AI agent in an IDE) can connect to the Figma MCP server in two primary ways:

#### 1. Desktop MCP Server

**Characteristics:**
- Runs **locally** through the Figma desktop app
- Enabled via **Dev Mode** in Figma
- Runs at local endpoint: `http://127.0.0.1:3845/mcp`

**Use Case:**  
Best for developers working on their local machines with direct access to Figma files.

**Setup:**
```bash
# Enable Dev Mode in Figma Desktop
# The MCP server automatically starts when Dev Mode is active

# Connect from your IDE's MCP client
MCP_ENDPOINT=http://127.0.0.1:3845/mcp
```

---

#### 2. Remote MCP Server

**Characteristics:**
- Connects directly to Figma's **globally hosted endpoint**
- Endpoint: `https://mcp.figma.com/mcp`
- No local Figma installation required

**Use Case:**  
Best for CI/CD pipelines, cloud-based development environments, or team collaboration.

**Setup:**
```bash
# Configure MCP client with remote endpoint
MCP_ENDPOINT=https://mcp.figma.com/mcp
FIGMA_ACCESS_TOKEN=your_figma_api_token
```

---

### Accessing Design Nodes

Once connected, the agent can access specific design nodes in **two ways**:

#### Selection-Based Access

The agent reads the user's **current selection** in the Figma desktop app.

**Workflow:**
```
1. Designer selects a frame/component in Figma
2. Agent queries: "Generate code for the selected design"
3. MCP server returns structured data for the current selection
4. Agent generates code
```

**Advantages:**
- Intuitive for designers
- No URL management needed
- Real-time iteration

---

#### Link-Based Access

Provide a **Figma URL** directly to the agent.

**Workflow:**
```
Developer: "Generate code for https://figma.com/file/abc123/node-id=456"
           ↓
Agent queries MCP server with URL
           ↓
MCP returns structured design data
           ↓
Agent generates code
```

**Advantages:**
- Works in automation/CI/CD
- Enables batch processing
- Version control integration

---

### Core Tools and Capabilities

The Figma MCP server exposes a set of **tools** to help LLMs translate designs into high-quality code. These capabilities move **far beyond simple "image-to-code" generation**.

#### 1. Generate Code from Selected Frames

**Description:**  
The primary design-to-code workflow, allowing an agent to transform a selected Figma frame into production code.

**Supported Outputs:**
- HTML/CSS
- React components
- Vue components
- React Native
- SwiftUI
- Flutter

**Example:**
```
Agent Tool Call: figma_mcp.generate_code()
Input: 
  - frame_id: "789:456"
  - target: "react"
  - style_system: "tailwind"

Output:
  - Component code with proper structure
  - Styling (CSS/Tailwind classes)
  - Asset references
  - Accessibility attributes
```

---

#### 2. Extract Design Context

**Description:**  
Pulls **design tokens**, **components**, and **layout data** directly from the Figma file into the IDE.

**Extracted Information:**
- **Design Tokens:**
  - Colors (with hex values)
  - Typography scales
  - Spacing units
  - Border radius values
  - Shadow definitions

- **Components:**
  - Component hierarchy
  - Props and variants
  - States (hover, active, disabled)
  - Component instances

- **Layout Data:**
  - Auto-layout constraints
  - Responsive breakpoints
  - Grid systems
  - Positioning rules

**Use Case:**
```
Scenario: Developer needs to implement a new feature consistent with 
          the existing design system

Agent Action:
1. Extract all color tokens from Figma → Generate colors.ts
2. Extract typography scale → Generate typography.css
3. Extract button component variants → Generate Button.tsx
4. Ensure new feature uses only extracted tokens
```

---

#### 3. Retrieve FigJam/Make Resources

**Description:**  
Enables agents to access and incorporate **early-stage ideas**, **user flows**, or **architecture maps** from FigJam and Make files into the development context.

**FigJam Use Cases:**
- User journey maps → Generate user story specs
- Wireframes → Generate initial component structure
- Brainstorming boards → Extract feature requirements

**Make Use Cases:**
- Architecture diagrams → Generate system design docs
- Database schemas → Generate data models
- API flow diagrams → Generate API specifications

**Workflow:**
```
Product Manager creates user flow in FigJam
           ↓
Agent extracts flow using Figma MCP
           ↓
Agent generates:
  - User stories (requirements.md)
  - Acceptance criteria
  - Test scenarios
           ↓
Agent passes to design team for high-fidelity mockups
```

---

#### 4. Code Connect Integration

**Description:**  
The **key to maintaining enterprise-grade UI consistency**. Code Connect ensures the generated code **reuses the developer's actual, existing components** from their codebase, rather than generating generic, non-system-compliant HTML/CSS.

**The Problem:**
Traditional image-to-code tools generate generic code:
```jsx
// ❌ Generic generated code - doesn't use design system
<div className="button-primary">
  <span>Click me</span>
</div>
```

**Code Connect Solution:**
```jsx
// ✅ Uses actual component from your codebase
import { Button } from '@company/design-system';

<Button variant="primary" size="large">
  Click me
</Button>
```

**How It Works:**

1. **Component Mapping:**  
   Developers create a mapping file that connects Figma components to actual code components.

```typescript
// button.figma.tsx - Code Connect mapping file
import { Button } from './Button';
import { figma } from '@figma/code-connect';

figma.connect(Button, 
  'https://figma.com/file/abc123?node-id=456:789', {
  props: {
    variant: figma.enum('Variant', {
      Primary: 'primary',
      Secondary: 'secondary',
      Tertiary: 'tertiary',
    }),
    size: figma.enum('Size', {
      Large: 'lg',
      Medium: 'md',
      Small: 'sm',
    }),
    children: figma.textContent('Label'),
  },
  example: props => <Button {...props} />
});
```

2. **Agent Code Generation:**  
   When the agent generates code from Figma, it **uses the Code Connect mappings** to ensure it references the correct production components.

**Benefits:**
- ✅ **Design System Compliance** - All generated code uses approved components
- ✅ **Type Safety** - Props match the actual TypeScript interfaces
- ✅ **Maintainability** - Updates to design system automatically propagate
- ✅ **Consistency** - No visual drift between design and implementation

**Enterprise FSI Use Case:**
```
Scenario: Bank has a strict design system with 50+ regulated components
          (forms, modals, data grids, etc.)

Code Connect ensures:
- All generated customer-facing UIs use approved components
- Accessibility standards (WCAG 2.1 AA) are automatically met
- Brand guidelines are enforced
- Compliance requirements (e.g., required disclosures) are included
```

---

### Best Practices: Quality In = Quality Out

The quality of AI-generated output is **directly proportional** to the quality of the design input.

#### 1. Structure Your Figma File for Better Code

**Naming Conventions:**
```
✅ Good:
  - "Button/Primary/Large/Enabled"
  - "Form/Input/TextField/Default"
  - "Card/Product/Desktop"

❌ Bad:
  - "Frame 123"
  - "Rectangle Copy 4"
  - "Group 7"
```

**Component Organization:**
- Use **consistent component variants** (not separate components)
- Apply **Auto Layout** for responsive behavior
- Define **constraints** explicitly
- Use **component properties** for dynamic content

**Design Tokens:**
- Create and use **Figma variables** for colors, spacing, typography
- Organize variables into collections (e.g., "Colors/Brand", "Spacing/Mobile")
- Use **semantic naming** (e.g., "color-primary-500" not "color-blue")

---

#### 2. Write Effective Prompts to Guide the AI

**Specific Instructions:**
```
✅ Good Prompt:
"Generate a React component for the selected card design. Use Tailwind CSS 
for styling, implement responsive breakpoints at 768px and 1024px, ensure 
WCAG 2.1 AA compliance, and use our Button component from the design system 
for the CTA."

❌ Vague Prompt:
"Make this into code"
```

**Context Provision:**
```
"This is a transaction history card for our banking app. It should:
- Display transaction amount, merchant, date, and category
- Support both debit (red) and credit (green) transactions
- Be clickable to show transaction details
- Work on mobile (375px) to desktop (1440px)
- Meet PCI DSS requirements (no sensitive data in DOM attributes)"
```

---

## The Testing Phase: Automated Test Generation

### The 'Testing MCP': A Framework for Agent-Driven Quality Assurance

The SDD and MCP paradigms fundamentally reshape quality assurance. **Testing is no longer an after-the-fact validation step** but an **integrated, automated consequence of the specification itself**. This can be conceptualized as a **"Testing MCP"**—a standardized interface for agents to perform a comprehensive suite of QA tasks.

**Key Paradigm Shift:**
```
Traditional: Spec → Code → Test (manual, often incomplete)
Agentic: Spec → Auto-Generate Tests → Code to Pass Tests → Continuous Validation
```

This section explores how AI agents transform every aspect of quality assurance, from functional testing to chaos engineering.

---

### A. From Spec to Test: Generating the QA Contract

In this new model, **SDD, BDD, and Test-Driven Development (TDD)** are part of a single, continuous workflow.

#### SDD Precedes BDD/TDD

**The Hierarchy:**

```
┌─────────────────────────────────────────┐
│  SDD (Spec-Driven Development)          │
│  The foundational "what" and "why"      │
│  - requirements.md                      │
│  - design.md (Figma)                    │
│  - architecture.md                      │
└─────────────┬───────────────────────────┘
              │
              ├─────────────────────────────┐
              │                             │
              ▼                             ▼
┌─────────────────────────┐   ┌─────────────────────────┐
│  BDD (Behavior-Driven)  │   │  TDD (Test-Driven)      │
│  Natural language       │   │  Code-level unit tests  │
│  Gherkin scenarios      │   │  Failing tests first    │
│  Business requirements  │   │  Red-Green-Refactor     │
└─────────────────────────┘   └─────────────────────────┘
```

**Key Insight:**  
SDD is the foundational "what" and "why". BDD, which focuses on **describing behavior in natural language**, and TDD, which focuses on **code-level unit tests**, are **derivatives** of that initial spec.

---

#### The Automated Workflow

The high-level specification becomes the **source for generating test contracts**. The workflow is:

**Step 1:** Human/Agent defines `spec.md` and Figma design

**Step 2:** Agent is prompted:  
```
"Based on get-user-profile.spec.md and the Figma MCP output, 
generate the BDD Gherkin scenarios"
```

**Step 3:** Agent is prompted:  
```
"Based on the technical plan, generate the failing TDD unit tests"
```

**Step 4:** Agent implements code to pass all tests

**Step 5:** Agent validates against original spec

---

#### The Gherkin Bridge

**Gherkin's Given-When-Then syntax** is the ideal **"ubiquitous language"** for this process. It creates a **human-readable, machine-executable contract** that bridges the gap between high-level business requirements and executable test code.

**Why Gherkin?**

✅ **Human-Readable** - Non-technical stakeholders can understand  
✅ **Machine-Executable** - Directly converts to test code  
✅ **Version-Controlled** - Lives alongside code in git  
✅ **Language-Agnostic** - Works with any programming language  
✅ **Bridges Communication Gap** - Technical and non-technical collaboration

**Impact:**  
This bridges the communication gap that causes a high percentage of software project failures, allowing technical and non-technical stakeholders to collaborate on a **shared understanding of behavior**.

---

### B. The 'Browser MCP': Agent-Driven UI Execution

The abstract concept of a "Testing MCP" is being made concrete through **"Browser MCP" servers**. These servers act as the bridge between an agent's natural language intent and the browser automation frameworks used for UI testing.

#### Selenium MCP Server

A prime example is the **Selenium MCP server**. This is a middleware component that acts as a **"bridge" or "translator"** between an AI agent (like Claude) and the Selenium WebDriver framework.

**Traditional Approach:**
```python
# Complex test script written by QA engineer
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
driver.get("https://example.com/login")
wait = WebDriverWait(driver, 10)

email_field = wait.until(EC.presence_of_element_located((By.ID, "email")))
email_field.send_keys("user@example.com")

password_field = driver.find_element(By.ID, "password")
password_field.send_keys("password123")

login_button = driver.find_element(By.CSS_SELECTOR, "button[type='submit']")
login_button.click()

# ... more complex code
```

**Agentic Approach with Selenium MCP:**
```
QA Engineer or Product Manager (in natural language):

"Navigate to the login page, enter 'user@example.com' in the email field, 
enter 'password123' in the password field, and click the login button"

↓ Selenium MCP Server translates ↓

Executable Selenium commands → Test execution
```

**Key Innovation:**  
Instead of writing complex test scripts, a QA engineer or even a product manager can issue **natural language commands**. The Selenium MCP server translates these commands into executable Selenium actions.

---

#### Enabled QA Workflows

This enables several powerful QA workflows:

**1. AI-Powered Test Execution**

Describe test steps in plain English and have the agent execute them:

```
Agent Prompt:
"Test the checkout flow:
1. Click the 'Add to Cart' button on the first product
2. Click the cart icon in the header
3. Click 'Proceed to Checkout'
4. Fill out the shipping form with test data
5. Select 'Credit Card' as payment method
6. Verify the order summary shows correct totals
7. Click 'Place Order'"

Agent executes via Selenium MCP → Full test run with screenshots
```

---

**2. Faster Debugging & Root Cause Analysis**

The server provides direct access to logs and test execution data:

```
Test Failure:
"Login test failed at step 3: Click login button"

Agent with Selenium MCP access:
- Retrieves browser console logs
- Captures network requests
- Takes screenshot at failure point
- Analyzes DOM state
- Provides RCA: "Login button was disabled due to email validation error"
```

---

**3. AI-Assisted Test Case Generation**

Agents can generate new test cases based on previous executions or specifications:

```
Agent Prompt:
"Analyze the last 10 login test executions and generate 5 new edge case 
test scenarios that weren't covered"

Agent Output (via Selenium MCP analysis):
1. Test login with email containing special characters (+, .)
2. Test login with maximum password length (128 characters)
3. Test login during session timeout
4. Test login with caps lock warning
5. Test login with autofill credentials
```

---

#### Community Implementations

A robust **open-source community** is building these servers, with notable implementations:

- **angiejones** - Selenium MCP with Claude Desktop integration
- **pshivapr** - Advanced Selenium MCP with visual debugging
- **Jyothishkumarav** - Enterprise Selenium MCP with CI/CD integration

---

#### Playwright MCP: Modern Alternative

**Playwright MCP** is emerging as a modern alternative, applying the same agentic principles to **Microsoft's Playwright framework**:

```
Advantages over Selenium:
✅ Built-in auto-waiting (no explicit waits needed)
✅ Better handling of modern web apps (SPAs, PWAs)
✅ Network interception and mocking
✅ Multiple browser contexts
✅ Video recording and trace files
✅ Better performance
```

**Example with Playwright MCP:**
```typescript
// Natural language → Playwright MCP → Executable test
Agent: "Test the login flow with network offline simulation"

Generated Playwright code:
await page.context().setOffline(true);
await page.goto('/login');
await expect(page.locator('.offline-message')).toBeVisible();
```

---

### Figma MCP as the Origin Point of Spec-Driven Testable UI

The Figma MCP server's **true architectural significance** is not just in code generation, but in its role as the **origin point for automated, design-first testing**.

**Key Insight:**  
The server provides **programmatic access** to all:
- UI elements
- Interactions
- Flow information
- Layout data

This structured data is **precisely what is needed to generate test cases**.

---

### Design-to-Test Workflow

An emerging class of tools can connect to Figma, extract flow and interaction information, and **automatically generate complete Behavior-Driven Development (BDD) test plans** in Gherkin syntax.

**Revolutionary Workflow:**
```
BDD scenarios are generated from the design 
BEFORE implementation code has been written
```

---

### Step-by-Step: From Figma to Automated Tests

#### Step 1: Design Contains Flow Information

**Example: User Login Flow in Figma**

```
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

---

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

---

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

---

#### Step 4: Generate Executable Test Code

From the Gherkin scenarios, generate actual test implementation:

```typescript
// Auto-generated from BDD scenarios using Playwright
import { test, expect } from '@playwright/test';

test.describe('User Authentication', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('Successful login with valid credentials', async ({ page }) => {
    // When I enter email
    await page.fill('[data-testid="email-input"]', 'user@example.com');
    
    // And I enter password
    await page.fill('[data-testid="password-input"]', 'SecurePass123!');
    
    // And I click Sign In
    await page.click('[data-testid="sign-in-button"]');
    
    // Then I should be redirected to Dashboard
    await expect(page).toHaveURL('/dashboard');
    
    // And I should see welcome message
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

  // ... additional test implementations
});
```

---

### Benefits of Design-First Testing

#### 1. Tests Written Before Code

**Traditional Workflow:**
```
Design → Code → Write Tests (often skipped due to time pressure)
```

**Agentic Workflow:**
```
Design → Auto-Generate Tests → Code to Pass Tests
```

**Result:**  
**100% test coverage from day one**, tests serve as the specification.

---

#### 2. Design as Living Documentation

The Figma file becomes the **single source of truth**:
- Product requirements
- Visual specification  
- Test scenarios
- Implementation checklist

**Version Control Integration:**
```
Each Figma version → New test suite generated
Developers can see exactly what changed and what tests need updating
```

---

#### 3. Compliance and Audit Trail

For FSI organizations:

**Audit Question:** "How do you ensure your login flow meets security requirements?"

**Answer with Evidence:**
```
1. Security requirements documented in Figma (annotations)
2. Test scenarios auto-generated from design (Gherkin files in git)
3. Test execution reports (Playwright HTML reports)
4. Implementation code (React components in git)

Complete trail: Requirement → Design → Test → Code
```

---

### C. A Multi-Faceted Agentic QA Strategy

This framework enables agents to automate a **multi-faceted UI testing strategy** that covers every aspect of quality assurance.

---

#### 1. Functional Verification

##### Agentic TDD (Test-Driven Development)

A developer can guide an agent, such as **Claude Code**, through the **"red-green-refactor" TDD loop**. This remains a **human-orchestrated process**, where the developer provides the **"critical architectural decisions and testing strategies"** and uses guardrails like pre-commit scripts to keep the agent on track.

**The TDD Loop with AI Agents:**

```
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

Agent refactors:
function calculateMonthlyPayment({ 
  principal, 
  annualRate, 
  years 
}: LoanParameters): number {
  if (principal <= 0) throw new Error('Principal must be positive');
  if (annualRate < 0) throw new Error('Rate cannot be negative');
  if (years <= 0) throw new Error('Years must be positive');
  
  const monthlyRate = annualRate / 12;
  const numPayments = years * 12;
  
  // Handle zero interest rate edge case
  if (annualRate === 0) {
    return principal / numPayments;
  }
  
  const payment = principal * 
    (monthlyRate * Math.pow(1 + monthlyRate, numPayments)) / 
    (Math.pow(1 + monthlyRate, numPayments) - 1);
    
  return Math.round(payment * 100) / 100; // Round to 2 decimals
}

Run test → ✅ PASSES (with better code)
```

**Guardrails for Agentic TDD:**

```bash
# Pre-commit hook to ensure tests pass
#!/bin/bash
npm test
if [ $? -ne 0 ]; then
  echo "❌ Tests failed. Commit blocked."
  exit 1
fi

npm run lint
if [ $? -ne 0 ]; then
  echo "❌ Linting failed. Commit blocked."
  exit 1
fi

echo "✅ All checks passed. Proceeding with commit."
```

---

##### Agentic BDD (Behavior-Driven Development)

To address the **"complexity of writing test cases,"** frameworks like **BDDTestAIGen** are emerging. These systems use LLMs and **"Agentic AI methods"** to automate the creation of BDD tests directly from high-level natural language requirements, **lowering the barrier to entry for all stakeholders**.

**Traditional BDD Challenge:**
```
Problem: Writing comprehensive Gherkin scenarios requires:
- Deep understanding of the application
- Knowledge of edge cases
- Consistent language and structure
- Significant time investment

Result: Incomplete test coverage, inconsistent scenarios
```

**BDDTestAIGen Solution:**

```
Input (Natural Language Requirement):
"Users should be able to transfer funds between their checking and savings 
accounts. The transfer should validate sufficient balance, apply daily 
transfer limits, and send confirmation notifications."

↓ BDDTestAIGen Agent ↓

Output (Complete Gherkin Scenarios):
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

  Scenario: Multiple transfers within limit
    When I transfer $500 from checking to savings
    And I transfer $300 from checking to savings
    Then both transfers should be completed
    And my checking balance should be $4200
    And my savings balance should be $10800

  Scenario: Transfer with zero amount
    When I transfer $0 from checking to savings
    Then the transfer should be rejected
    And I should see error "Amount must be greater than zero"

  Scenario: Transfer to same account
    When I transfer $100 from checking to checking
    Then the transfer should be rejected
    And I should see error "Cannot transfer to the same account"
```

**Key Benefits:**

✅ **Comprehensive Coverage** - Agent generates scenarios you might miss  
✅ **Consistent Structure** - All scenarios follow the same pattern  
✅ **Edge Case Discovery** - Agent thinks of boundary conditions  
✅ **Fast Iteration** - Generate scenarios in seconds, not hours  
✅ **Stakeholder Accessibility** - Non-technical team members can contribute requirements

---

#### 2. Visual Validation: Beyond Pixel-Perfect Testing

This moves beyond traditional, **brittle pixel-to-pixel snapshot testing**. AI-native testing platforms like **Mabl** employ AI for **"visual validation"**. This allows the test to **understand the intent of the UI** and validate **component-level correctness** rather than just pixels, significantly **reducing false positives** from minor, non-breaking changes.

**Traditional Visual Testing:**
```
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

```
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
- Intentional design updates flagged as expected changes

Result: <5% false positive rate → Tests are trusted
```

**Example:**

```typescript
// Traditional snapshot test (brittle)
test('Login form matches snapshot', async ({ page }) => {
  await page.goto('/login');
  await expect(page).toHaveScreenshot('login.png');
  // Fails if ANY pixel changes
});

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
    tolerance: 'semantic' // Understands intent, not pixels
  });
  
  // Passes even if:
  // - Button color changes from #007bff to #0056b3
  // - Padding changes by 2px
  // - Font changes from Arial to Helvetica
  // - Dynamic timestamp updates
});
```

**FSI Use Case:**

```
Banking Dashboard Visual Validation:
✅ Validates account cards display correct data structure
✅ Confirms transaction list maintains chronological order
✅ Ensures compliance disclaimers are visible
✅ Validates responsive layout across devices

Ignores:
- Exact account balances (test data varies)
- Transaction timestamps (dynamic)
- Minor branding updates
- A/B test variations
```

---

#### 3. Accessibility Testing: Automated WCAG Compliance

Accessibility is **no longer a separate, manual audit**. Modern AI test tools integrate **automated WCAG-based validation** directly into the test execution flow. An agent can be tasked to automatically check for accessibility compliance, including **color contrast, correct ARIA role implementation, and keyboard navigation**, without requiring developers to write or maintain a separate suite of accessibility rules.

**The Problem with Traditional A11y Testing:**
```
❌ Manual audits (expensive, infrequent)
❌ Separate test suites (out of sync with main tests)
❌ Requires specialized expertise
❌ Often tested at the end (too late to fix easily)
❌ Incomplete coverage
```

**Agentic Accessibility Testing:**

```
✅ Automated, continuous validation
✅ Integrated with functional tests
✅ AI understands context (not just rule violations)
✅ Tested throughout development
✅ 100% coverage
```

**Implementation:**

```typescript
// Integrated A11y testing with Playwright + axe-core
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test.describe('Login Page Accessibility', () => {
  test('should not have any automatically detectable WCAG violations', async ({ page }) => {
    await page.goto('/login');
    
    const accessibilityScanResults = await new AxeBuilder({ page })
      .withTags(['wcag2a', 'wcag2aa', 'wcag21a', 'wcag21aa'])
      .analyze();
    
    expect(accessibilityScanResults.violations).toEqual([]);
  });
  
  test('should support full keyboard navigation', async ({ page }) => {
    await page.goto('/login');
    
    // Tab through all focusable elements
    await page.keyboard.press('Tab'); // Email input
    await expect(page.locator('[data-testid="email-input"]')).toBeFocused();
    
    await page.keyboard.press('Tab'); // Password input
    await expect(page.locator('[data-testid="password-input"]')).toBeFocused();
    
    await page.keyboard.press('Tab'); // Remember me checkbox
    await expect(page.locator('[data-testid="remember-me-checkbox"]')).toBeFocused();
    
    await page.keyboard.press('Tab'); // Sign in button
    await expect(page.locator('[data-testid="sign-in-button"]')).toBeFocused();
    
    // Submit form with Enter key
    await page.keyboard.press('Enter');
  });
  
  test('should have proper ARIA labels and roles', async ({ page }) => {
    await page.goto('/login');
    
    // Check for proper form labeling
    const emailInput = page.locator('input[type="email"]');
    await expect(emailInput).toHaveAttribute('aria-label', /email/i);
    
    const passwordInput = page.locator('input[type="password"]');
    await expect(passwordInput).toHaveAttribute('aria-label', /password/i);
    
    // Check for error message association
    await page.fill('[data-testid="email-input"]', 'invalid');
    await page.click('[data-testid="sign-in-button"]');
    
    const errorMessage = page.locator('[data-testid="email-error"]');
    const errorId = await errorMessage.getAttribute('id');
    await expect(emailInput).toHaveAttribute('aria-describedby', errorId);
  });
  
  test('should meet color contrast requirements', async ({ page }) => {
    await page.goto('/login');
    
    const results = await new AxeBuilder({ page })
      .withTags(['cat.color'])
      .analyze();
    
    expect(results.violations.filter(v => v.id === 'color-contrast')).toEqual([]);
  });
});
```

**Agent-Driven A11y Testing:**

```
Developer: "Validate accessibility for the transaction dashboard"

Agent (using Playwright + axe-core):
1. Runs automated WCAG 2.1 AA scan
2. Tests keyboard navigation paths
3. Validates ARIA attributes
4. Checks color contrast ratios
5. Tests screen reader compatibility
6. Validates focus management
7. Tests with zoom (200%, 400%)

Agent Report:
"✅ All WCAG 2.1 AA criteria met
 ✅ Full keyboard navigation supported
 ✅ Proper ARIA attributes present
 ⚠️  Warning: One link has ambiguous text ('Click here')
    Recommendation: Change to 'View transaction details'"
```

**FSI Compliance Benefits:**

```
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

### D. Beyond the UI: Non-Functional and Chaos Testing

A comprehensive agentic QA strategy must extend beyond UI functionality to encompass the **non-functional requirements** that define an application's **production-readiness**. An agent can be tasked, as part of the SDD plan, to generate and execute non-functional tests for:

#### 1. Performance and Load Testing

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
    And no requests should timeout

  Scenario: Graceful degradation under extreme load
    Given the system is in a healthy state
    When 5000 concurrent users request transaction history
    Then the system should return 429 (Too Many Requests) for excess traffic
    And existing users should maintain sub-1s response times
    And no data corruption should occur
```

**Agent Implementation:**

```typescript
// Agent generates k6 load test from Gherkin
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

#### 2. Security and Vulnerability Testing

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
    Given I have a valid API token
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

#### 3. Scalability and Volume Testing

```gherkin
Feature: Database Scalability
  As a database administrator
  I want to ensure the database can handle growing data volumes
  So that performance doesn't degrade over time

  Scenario: Query performance with 10M transaction records
    Given the transactions table contains 10 million records
    When I query transactions for a user with 1000 transactions
    Then the query should complete in under 100ms
    And the query plan should use the account_id index

  Scenario: Write performance under high volume
    Given the system is processing 10,000 transactions per second
    When I insert a new transaction
    Then the write should complete in under 50ms
    And no write conflicts should occur
```

---

### E. Agentic Chaos Engineering: The Ultimate Testing MCP

The most advanced application of this framework is **Agentic Chaos Engineering**. This moves chaos engineering from a **periodic, manual "gameday" event** into an **automated, continuous CI gate**.

**Traditional Chaos Engineering:**
```
❌ Manual execution (quarterly "gamedays")
❌ Requires dedicated team
❌ Disruptive to development workflow
❌ Limited coverage (only tests what humans think of)
❌ Results hard to track over time
```

**Agentic Chaos Engineering:**
```
✅ Automated, continuous execution
✅ Integrated into CI/CD pipeline
✅ Non-disruptive (runs in isolated environments)
✅ Comprehensive coverage (AI generates scenarios)
✅ Tracked as code in version control
```

---

#### BDD for Resilience Testing

This is achieved by applying the **BDD workflow to resilience testing**. An agent in a CI/CD pipeline can be given a human-readable chaos experiment written in **Gherkin**:

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
    When the agent executes an AWS Fault Injection Simulator (FIS) experiment 
      to "terminate 10% of EKS pods"
    Then the system availability should remain above 99.9%
    And the CloudWatch "PodCountLow" alarm should fire within 1 minute
    And Kubernetes should auto-heal by creating replacement pods within 2 minutes
    And no customer-facing errors should be logged
    And API response times should remain under 500ms

  Scenario: Survive availability zone failure
    When the agent executes an AWS FIS experiment 
      to "make us-east-1a unavailable"
    Then traffic should automatically route to us-east-1b and us-east-1c
    And system availability should remain above 99.5%
    And the CloudWatch "AZDown" alarm should fire within 30 seconds
    And all active user sessions should be preserved

  Scenario: Survive database primary failover
    When the agent executes an AWS FIS experiment 
      to "force RDS primary failover"
    Then the application should detect the failover within 10 seconds
    And reconnect to the new primary within 15 seconds
    And in-flight transactions should be retried automatically
    And no data loss should occur

  Scenario: Survive network latency injection
    When the agent injects 500ms latency on 50% of network calls
    Then the application should implement circuit breakers
    And fallback responses should be served within 200ms
    And user-facing error rate should remain under 0.1%
    And timeout configurations should prevent request queueing

  Scenario: Survive memory pressure
    When the agent injects a memory leak causing 80% memory usage
    Then Kubernetes should detect the unhealthy pod
    And replace the pod before it reaches 90% memory
    And no cascading failures should occur
    And active requests should drain gracefully
```

---

#### Agent Implementation

**The agent is no longer just testing a button; it is programmatically executing controlled fault injections and validating the system's architectural resilience against the specification.**

```typescript
// Agent-generated chaos test implementation
import { test, expect } from '@playwright/test';
import { FISClient, StartExperimentCommand } from '@aws-sdk/client-fis';
import { CloudWatchClient, DescribeAlarmsCommand } from '@aws-sdk/client-cloudwatch';
import { EKSClient, DescribeNodegroupCommand } from '@aws-sdk/client-eks';

test.describe('EKS Pod Resilience', () => {
  let fisClient: FISClient;
  let cwClient: CloudWatchClient;
  let eksClient: EKSClient;

  test.beforeAll(() => {
    fisClient = new FISClient({ region: 'us-east-1' });
    cwClient = new CloudWatchClient({ region: 'us-east-1' });
    eksClient = new EKSClient({ region: 'us-east-1' });
  });

  test('Survive partial pod failure', async () => {
    // Verify baseline health
    const baselineHealth = await checkSystemHealth();
    expect(baselineHealth.availability).toBeGreaterThan(99.9);

    // Execute chaos experiment
    const experiment = await fisClient.send(new StartExperimentCommand({
      experimentTemplateId: 'EXP-Pod-Termination-10pct',
      tags: { 'Test': 'Automated', 'Agent': 'CI-CD' }
    }));

    // Monitor during experiment
    const monitoring = monitorSystem(experiment.experiment.id, {
      duration: 300, // 5 minutes
      metrics: ['availability', 'error_rate', 'response_time', 'pod_count']
    });

    // Validate availability
    const availabilityDuringChaos = await monitoring.getMetric('availability');
    expect(availabilityDuringChaos.min).toBeGreaterThan(99.9);

    // Validate alarm fired
    const alarms = await cwClient.send(new DescribeAlarmsCommand({
      AlarmNames: ['PodCountLow']
    }));
    const alarmHistory = alarms.MetricAlarms[0].StateUpdatedTimestamp;
    expect(alarmHistory).toBeDefined();
    expect(Date.now() - alarmHistory.getTime()).toBeLessThan(60000); // Within 1 min

    // Validate auto-healing
    const podsAfterChaos = await eksClient.send(new DescribeNodegroupCommand({
      clusterName: 'production',
      nodegroupName: 'app-nodes'
    }));
    expect(podsAfterChaos.nodegroup.scalingConfig.desiredSize)
      .toBe(baselineHealth.podCount);

    // Validate no customer impact
    const errorLogs = await queryCloudWatchLogs({
      logGroup: '/aws/application/errors',
      timeRange: [experiment.startTime, experiment.endTime],
      filter: 'level=ERROR AND customer_facing=true'
    });
    expect(errorLogs.length).toBe(0);

    // Validate response times
    const responseTime = await monitoring.getMetric('response_time');
    expect(responseTime.p95).toBeLessThan(500);
  });
});
```

---

#### CI/CD Integration

```yaml
# .github/workflows/chaos-testing.yml
name: Continuous Chaos Engineering

on:
  schedule:
    - cron: '0 2 * * *' # Daily at 2 AM
  workflow_dispatch:

jobs:
  chaos-tests:
    runs-on: ubuntu-latest
    environment: staging
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Run Agentic Chaos Tests
        run: npx playwright test chaos/*.spec.ts
        env:
          AWS_REGION: us-east-1
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          ENVIRONMENT: staging
      
      - name: Upload Chaos Test Report
        if: always()
        uses: actions/upload-artifact@v3
        with:
          name: chaos-test-results
          path: playwright-report/
      
      - name: Notify on Failure
        if: failure()
        uses: slackapi/slack-github-action@v1
        with:
          payload: |
            {
              "text": "🔥 Chaos test failed! System resilience below threshold."
            }
```

---

#### Benefits for FSI Organizations

**Regulatory Compliance:**
```
Financial regulators require proof of:
✅ Business continuity planning
✅ Disaster recovery capabilities
✅ Resilience testing

Agentic Chaos Engineering provides:
✅ Automated, documented resilience tests
✅ Version-controlled chaos scenarios
✅ Continuous validation of recovery procedures
✅ Audit trail of all chaos experiments
```

**Operational Excellence:**
```
Traditional chaos gamedays:
- Once per quarter
- 2-hour disruption
- Tests 5-10 scenarios
- Manual documentation

Agentic chaos testing:
- Daily automated runs
- Zero disruption (isolated environments)
- Tests 50+ scenarios
- Automatic documentation
- Trend analysis over time
```

**Real Example:**

```
Major Bank Scenario:
"Prove payment processing survives AWS region failure"

Traditional Approach:
- Schedule gameday (3 weeks planning)
- Coordinate with 20+ teams
- 4-hour event on Saturday
- Manual failover execution
- Document results in wiki

Agentic Approach:
- Write Gherkin scenario (30 minutes)
- Agent generates test code
- Runs nightly in staging
- Automatic multi-region failover validation
- Results tracked in git
- Alerts on degradation

Result: 100x more frequent testing, zero coordination overhead
```

---

### Summary: The Complete Testing MCP

This framework represents the **full realization of a "Testing MCP"** that validates the entire system:

```
┌─────────────────────────────────────────────────────┐
│  Spec (Figma + requirements.md)                     │
└─────────────────┬───────────────────────────────────┘
                  │
    ┌─────────────┴─────────────┬──────────────────┬──────────────┐
    │                           │                  │              │
    ▼                           ▼                  ▼              ▼
┌─────────┐              ┌───────────┐      ┌──────────┐   ┌──────────┐
│   BDD   │              │    TDD    │      │ Visual   │   │   A11y   │
│ Gherkin │              │ Unit Tests│      │ Testing  │   │  Testing │
└─────────┘              └───────────┘      └──────────┘   └──────────┘
    │                           │                  │              │
    └─────────────┬─────────────┴──────────────────┴──────────────┘
                  │
                  ▼
        ┌─────────────────────┐
        │  Browser MCP        │
        │  (Selenium/         │
        │   Playwright)       │
        └──────────┬──────────┘
                   │
     ┌─────────────┼─────────────┬──────────────────┐
     │             │             │                  │
     ▼             ▼             ▼                  ▼
┌──────────┐  ┌──────────┐  ┌──────────┐     ┌──────────┐
│Performance│  │ Security │  │Scalability│     │  Chaos   │
│   Tests   │  │  Tests   │  │  Tests   │     │   Eng    │
└──────────┘  └──────────┘  └──────────┘     └──────────┘
     │             │             │                  │
     └─────────────┴─────────────┴──────────────────┘
                   │
                   ▼
          ┌────────────────┐
          │  CI/CD Gate    │
          │  All tests     │
          │  must pass     │
          └────────────────┘
                   │
                   ▼
          ┌────────────────┐
          │  Production    │
          │  Deploy        │
          └────────────────┘
```

**From UI behavior to architectural integrity, the Testing MCP ensures complete system quality through automated, agent-driven validation.**

---

## The Execution Phase: Agentic Code Generation

### From Tests to Implementation

With **tests generated from design**, AI agents now have:
1. **Visual specification** (Figma MCP)
2. **Functional specification** (BDD scenarios)
3. **Test harness** (Executable tests)

The agent's job: **Write code that passes all tests**.

---

### Test-Driven Agentic Development (TDAD)

**Workflow:**

```
┌──────────────────────────────────────────────────────┐
│  1. Agent reads Figma design via MCP                 │
│     - Extracts components, layout, styling           │
└────────────────┬─────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────┐
│  2. Agent reads generated test scenarios             │
│     - Understands expected behavior                  │
│     - Identifies edge cases                          │
└────────────────┬─────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────┐
│  3. Agent generates initial implementation           │
│     - Creates component structure                    │
│     - Implements styling from Figma                  │
│     - Uses Code Connect mappings                     │
└────────────────┬─────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────┐
│  4. Agent runs tests                                 │
│     - Executes Playwright/Jest tests                 │
│     - Captures failures                              │
└────────────────┬─────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────┐
│  5. Agent iterates until all tests pass              │
│     - Analyzes test failures                         │
│     - Refines implementation                         │
│     - Re-runs tests                                  │
│     - Repeats until 100% pass rate                   │
└────────────────┬─────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────┐
│  6. Agent creates pull request                       │
│     - Generated code                                 │
│     - Test results                                   │
│     - Design-to-code traceability                    │
└──────────────────────────────────────────────────────┘
```

---

### Agent Execution Tools

The AI agent uses standard development tools:

#### Code Generation
- **Claude Agent SDK** - Core orchestration
- **Claude Code** - IDE-integrated coding agent
- **GitHub Copilot** - Alternative agent

#### Testing Frameworks
- **Playwright** - End-to-end testing
- **Jest** - Unit testing
- **Cypress** - Component testing
- **Storybook** - Visual regression testing

#### Quality Assurance
- **ESLint** - Code quality
- **Prettier** - Code formatting
- **TypeScript** - Type checking
- **Lighthouse** - Performance and accessibility auditing

---

### Example: Agent-Generated Implementation

**Input to Agent:**

```
Design: Figma URL for Login component
Tests: login.spec.ts (generated from Figma flows)
Context: 
  - Use React + TypeScript
  - Use Tailwind CSS
  - Use @company/design-system components
  - Must pass all 12 test scenarios
```

**Agent Output:**

```tsx
// Login.tsx - Generated by AI agent
import React, { useState } from 'react';
import { Button, Input, Checkbox, Link } from '@company/design-system';
import { useAuth } from '@/hooks/useAuth';
import { validateEmail } from '@/utils/validation';

interface LoginProps {
  onSuccess?: () => void;
}

export const Login: React.FC<LoginProps> = ({ onSuccess }) => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [rememberMe, setRememberMe] = useState(false);
  const [errors, setErrors] = useState<{ email?: string; password?: string }>({});
  
  const { login, isLoading } = useAuth();

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    
    // Validate email format
    if (!validateEmail(email)) {
      setErrors({ email: 'Please enter a valid email address' });
      return;
    }
    
    try {
      await login(email, password, rememberMe);
      onSuccess?.();
    } catch (error) {
      setErrors({ password: 'Invalid email or password' });
      setPassword(''); // Clear password on error
    }
  };

  return (
    <form onSubmit={handleSubmit} className="w-full max-w-md space-y-6">
      <Input
        data-testid="email-input"
        type="email"
        label="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        error={errors.email}
        required
        autoComplete={rememberMe ? 'email' : 'off'}
      />
      
      <Input
        data-testid="password-input"
        type="password"
        label="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        error={errors.password}
        required
      />
      
      <Checkbox
        data-testid="remember-me-checkbox"
        checked={rememberMe}
        onChange={(checked) => setRememberMe(checked)}
        label="Remember me"
      />
      
      <Button
        data-testid="sign-in-button"
        type="submit"
        variant="primary"
        size="large"
        fullWidth
        loading={isLoading}
      >
        Sign In
      </Button>
      
      <Link
        data-testid="forgot-password-link"
        href="/password-reset"
        variant="secondary"
      >
        Forgot password?
      </Link>
    </form>
  );
};
```

**Key Observations:**

✅ Uses actual components from `@company/design-system`  
✅ Implements all test scenarios (email validation, error handling, remember me)  
✅ Includes proper `data-testid` attributes for testing  
✅ Type-safe with TypeScript  
✅ Accessible (labels, ARIA attributes via design system)  
✅ Matches Figma design (layout, spacing, styling)

**Test Results:**
```bash
$ npm test login.spec.ts

PASS  src/components/Login.spec.ts
  User Authentication
    ✓ Successful login with valid credentials (234ms)
    ✓ Failed login with invalid email format (89ms)
    ✓ Failed login with wrong password (112ms)
    ✓ Password reset flow (67ms)
    ✓ Remember me functionality (145ms)
    ... (12/12 tests passed)

Test Suites: 1 passed, 1 total
Tests:       12 passed, 12 total
Time:        2.847s
```

---

## Complete Workflow Example

### Scenario: Building a Banking Transaction Dashboard

Let's walk through the complete workflow for a real FSI use case.

---

### Phase 1: Design (Week 1)

**Designers' Work:**

1. **Create User Flow in FigJam:**
   - User logs in
   - Views account overview
   - Filters transactions by date/type
   - Exports transaction history

2. **Create High-Fidelity Design in Figma:**
   - Dashboard layout with account cards
   - Transaction list component
   - Filter panel
   - Export modal

3. **Apply Design System:**
   - Use Code Connect-mapped components
   - Apply design tokens (colors, spacing, typography)
   - Define responsive breakpoints
   - Add interaction states

**Figma File Structure:**
```
Banking App/
├─ Flows/
│  └─ Transaction Dashboard Flow (FigJam)
├─ Components/
│  ├─ AccountCard
│  ├─ TransactionListItem
│  ├─ FilterPanel
│  └─ ExportModal
└─ Screens/
   ├─ Dashboard/Desktop
   ├─ Dashboard/Tablet
   └─ Dashboard/Mobile
```

---

### Phase 2: Test Generation (Day 1 of Week 2)

**Agent Process:**

```bash
# Agent connects to Figma MCP
agent> connect_figma_mcp --file "banking-app" --screen "Dashboard"

# Extract flows
agent> extract_flows --source "FigJam/Transaction Dashboard Flow"

# Generate BDD scenarios
agent> generate_bdd_tests --output "tests/features/dashboard.feature"
```

**Generated Gherkin:**

```gherkin
Feature: Transaction Dashboard
  As a banking customer
  I want to view and filter my transaction history
  So that I can manage my finances effectively

  Scenario: View account overview
    Given I am logged in to my account
    When I navigate to the dashboard
    Then I should see my checking account balance
    And I should see my savings account balance
    And I should see the last 10 transactions

  Scenario: Filter transactions by date range
    Given I am on the dashboard
    When I click the "Filter" button
    And I select start date "2025-01-01"
    And I select end date "2025-01-31"
    And I click "Apply Filters"
    Then I should see only transactions between those dates
    And the transaction count should update

  Scenario: Export transaction history as CSV
    Given I am on the dashboard with transactions visible
    When I click the "Export" button
    And I select format "CSV"
    And I click "Download"
    Then a CSV file should be downloaded
    And the file should contain all visible transactions

  # ... (20+ additional scenarios for edge cases, errors, accessibility)
```

**Generate Executable Tests:**

```bash
agent> generate_test_implementation --framework playwright --output "tests/e2e/"
```

---

### Phase 3: Implementation (Days 2-3 of Week 2)

**Agent Process:**

```bash
# Generate components from Figma
agent> generate_code --figma-screen "Dashboard/Desktop" 
                     --framework react 
                     --output "src/components/Dashboard"

# Run tests
agent> run_tests --watch

# Agent iterates:
# - Test fails: "Filter panel not responsive on mobile"
# - Agent analyzes Figma mobile design
# - Agent refines implementation
# - Test passes

# Continue until all tests pass
```

**Final Implementation Structure:**

```
src/
├─ components/
│  ├─ Dashboard/
│  │  ├─ Dashboard.tsx (main component)
│  │  ├─ AccountCard.tsx
│  │  ├─ TransactionList.tsx
│  │  ├─ FilterPanel.tsx
│  │  └─ ExportModal.tsx
│  └─ __tests__/
│     └─ Dashboard.spec.tsx
├─ hooks/
│  ├─ useTransactions.ts
│  └─ useExport.ts
└─ utils/
   └─ formatCurrency.ts
```

---

### Phase 4: Review & Deploy (Days 4-5 of Week 2)

**Pull Request Created by Agent:**

```markdown
## 🤖 AI-Generated: Transaction Dashboard

**Figma Design:** https://figma.com/file/banking-app/Dashboard
**Test Coverage:** 100% (24/24 scenarios passing)

### Changes
- ✅ Implemented Dashboard component with account overview
- ✅ Built transaction filtering (date, type, amount)
- ✅ Added CSV/PDF export functionality
- ✅ Responsive design (mobile, tablet, desktop)
- ✅ WCAG 2.1 AA compliant
- ✅ All Code Connect components used correctly

### Test Results
```
PASS  tests/e2e/dashboard.spec.ts (24 tests, 2.3s)
PASS  tests/unit/useTransactions.spec.ts (12 tests, 0.4s)
PASS  tests/a11y/dashboard.a11y.spec.ts (8 tests, 1.1s)
```

### Screenshots
[Automated visual regression tests show pixel-perfect match to Figma]

### Audit Trail
- Design Spec: Figma file v3.2
- Test Spec: dashboard.feature (git hash: abc123)
- Implementation: (this PR)
```

**Human Review:**
- Product Manager: ✅ Matches requirements
- Designer: ✅ Matches design
- QA: ✅ All tests passing
- Security: ✅ No sensitive data exposure
- Developer: ✅ Code quality, maintainability

**Deploy to Production:**
```bash
$ git merge ai-generated/dashboard
$ npm run deploy:production
```

---

### Outcome

**Timeline:**
- Week 1: Design (human)
- Week 2, Day 1: Test generation (automated)
- Week 2, Days 2-3: Implementation (AI agent)
- Week 2, Days 4-5: Review and deploy
- **Total: 1.5 weeks from design to production**

**Traditional Timeline:**
- Week 1-2: Design
- Week 3: Write test plan
- Week 4-6: Implementation
- Week 7: Testing and bug fixes
- Week 8: Deploy
- **Total: 8 weeks**

**Improvement: 5.3x faster**

---

## Best Practices

### 1. Design Best Practices

#### Component Naming
```
✅ Use semantic, hierarchical names:
   "Button/Primary/Large/Enabled"
   
❌ Avoid generic names:
   "Frame 123", "Group 45"
```

#### Design Tokens
```
✅ Create Figma variables for all design tokens
✅ Use semantic naming (color-primary-500, not color-blue)
✅ Organize into collections (Colors, Spacing, Typography)
```

#### Code Connect Mappings
```
✅ Map all reusable components
✅ Document props and variants
✅ Keep mappings in sync with design system updates
```

---

### 2. Testing Best Practices

#### Comprehensive Coverage
```
✅ Test happy paths
✅ Test error states
✅ Test edge cases
✅ Test accessibility
✅ Test responsive behavior
```

#### Meaningful Scenarios
```gherkin
✅ Good:
Feature: User can securely export transaction history
  Scenario: Export includes only transactions from selected date range

❌ Too vague:
Feature: Export works
  Scenario: User exports data
```

---

### 3. Agent Execution Best Practices

#### Clear Instructions
```
✅ "Generate a React component using our design system components,
    ensure WCAG 2.1 AA compliance, and match the Figma design at
    https://figma.com/file/abc123"

❌ "Make the design into code"
```

#### Iterative Refinement
```
✅ Let agent iterate to pass all tests
✅ Review agent's reasoning for failures
✅ Provide additional context if agent gets stuck
```

#### Human Oversight
```
✅ Always review generated code
✅ Verify security and compliance
✅ Check for performance issues
✅ Ensure maintainability
```

---

## Integration with SDD Methodology

This design-first workflow is fully compatible with the **Agentic Spec-Driven Development (SDD)** methodology described in the main README.

### Mapping to SDD Artifacts

| SDD Artifact | Design-First Equivalent |
|--------------|-------------------------|
| `requirements.md` | FigJam user flows + Figma annotations |
| `design.md` | Figma design file + Code Connect mappings |
| `tasks.md` | Auto-generated from test scenarios |
| Code implementation | AI agent execution guided by tests |

### SDD Tools Integration

#### AWS Kiro
- Import Figma designs directly
- Auto-generate specs from designs
- Link tests to design elements

#### GitHub Spec Kit
```bash
/speckit.design --figma-url https://figma.com/file/abc123
# Automatically generates constitute.md, specify.md, plan.md from Figma

/speckit.test --generate-bdd
# Creates BDD scenarios from design flows

/speckit.implement
# Agent implements code to pass tests
```

#### Agent OS
```
Standards/ ← Design system guidelines, Code Connect mappings
Product/   ← Figma files, user flows
Specs/     ← Auto-generated from Figma → BDD → implementation
```

---

## Tools and Resources

### Figma Tools

- **Figma Desktop App** - Local MCP server
- **Figma Dev Mode** - Enables MCP server
- **Code Connect** - Component mapping tool
- **Figma API** - Programmatic access

**Documentation:**
- [Figma MCP Server Docs](https://www.figma.com/mcp)
- [Code Connect Guide](https://www.figma.com/code-connect)

---

### Testing Frameworks

- **Playwright** - E2E testing ([docs](https://playwright.dev))
- **Jest** - Unit testing ([docs](https://jestjs.io))
- **Cucumber** - BDD framework ([docs](https://cucumber.io))
- **Storybook** - Component testing ([docs](https://storybook.js.org))

---

### AI Agents

- **Claude Agent SDK** - Anthropic's agent framework ([docs](https://docs.anthropic.com/agent-sdk))
- **Claude Code** - IDE-integrated coding agent
- **GitHub Copilot** - AI pair programmer
- **Amazon Q Developer** - AWS coding agent

---

### MCP Ecosystem

- **Model Context Protocol** - Open standard ([spec](https://spec.modelcontextprotocol.io))
- **Azure MCP Server** - Microsoft's MCP implementation
- **Custom MCP Servers** - Build your own tool integrations

---

## Conclusion

The **Design → Test → Execute** workflow represents the future of enterprise software development:

1. **Designers create visual specifications** in Figma
2. **AI agents auto-generate comprehensive tests** from designs
3. **AI agents write code** to pass the tests
4. **Humans review and deploy** with full confidence

For Financial Services organizations, this approach provides:

✅ **10x faster development cycles**  
✅ **100% test coverage from day one**  
✅ **Complete design-to-code audit trail**  
✅ **Automatic design system compliance**  
✅ **Reduced technical debt**  
✅ **Higher quality, more consistent applications**

---

## Next Steps

1. **Set up Figma MCP**
   - Enable Dev Mode in Figma
   - Configure MCP client in your IDE
   - Test connection with sample design

2. **Create Code Connect Mappings**
   - Map your design system components
   - Test with simple component first
   - Expand to full design system

3. **Generate First Test Suite**
   - Start with simple user flow
   - Generate BDD scenarios
   - Implement executable tests

4. **Run Agent-Driven Implementation**
   - Provide Figma + tests to agent
   - Let agent iterate to pass tests
   - Review and refine

5. **Integrate with SDD Process**
   - Adopt AWS Kiro, GitHub Spec Kit, or Agent OS
   - Establish team workflows
   - Train developers and designers

---

## Additional Resources

- [Main README](README.md) - Complete implementation blueprint
- [FSI Guide](AGENTIC_BANK_FSI.md) - Agentic AI for Financial Services
- [Figma MCP Documentation](https://www.figma.com/mcp)
- [Model Context Protocol Spec](https://spec.modelcontextprotocol.io)
- [Claude Agent SDK](https://docs.anthropic.com/agent-sdk)

---

*Last Updated: November 16, 2025*
