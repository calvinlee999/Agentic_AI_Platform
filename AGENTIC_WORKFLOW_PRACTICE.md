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
