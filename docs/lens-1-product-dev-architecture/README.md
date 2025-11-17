# Lens 1: Product Development & Architecture

## Overview

The Product Development & Architecture lens provides a comprehensive framework for building enterprise applications using AI-driven development methodologies. This module covers the complete journey from design specifications to production code, leveraging the power of Spec-Driven Development (SDD), Model Context Protocol (MCP), and modern agent orchestration.

## Core Technology Stack

| Technology | Purpose | Application |
|-----------|---------|-------------|
| **Figma MCP** | Design-to-development bridge | Visual specs, design system access |
| **Amazon Q Developer** | AWS-native agent | Cloud development assistance |
| **SDD (Spec-Driven Development)** | Methodology | Requirements as executable artifacts |
| **AWS MCP Marketplace** | Ecosystem | Enterprise tool integrations |
| **Claude Agentic SDK** | AI orchestration | Tool use, multi-step reasoning |
| **LangChain** | Agent framework | Multi-agent orchestration |
| **GitHub Copilot** | IDE assistant | Code completion, pair programming |
| **Claude Code** | Deep-context agent | Complex refactoring, analysis |

## Table of Contents

1. [Introduction: The Agentic Development Paradigm](#1-introduction-the-agentic-development-paradigm)
2. [Spec-Driven Development (SDD) Foundations](#2-spec-driven-development-sdd-foundations)
3. [Model Context Protocol (MCP): Universal Integration](#3-model-context-protocol-mcp-universal-integration)
4. [The Design Phase: Figma MCP](#4-the-design-phase-figma-mcp)
5. [The Execution Phase: Agentic Code Generation](#5-the-execution-phase-agentic-code-generation)
6. [Agent Comparison & Selection](#6-agent-comparison--selection)
7. [Best Practices & Patterns](#7-best-practices--patterns)
8. [FSI Use Cases](#8-fsi-use-cases)

---

## 1. Introduction: The Agentic Development Paradigm

The integration of AI agents into software development represents a fundamental shift from traditional coding practices to a specification-first, agent-driven approach. This paradigm shift requires:

- **Specifications as Source of Truth**: Moving from "vibe coding" to verifiable intent
- **Universal Integration Standards**: MCP as the "USB-C for AI"
- **Multi-Agent Orchestration**: Right agent for the right task
- **Design-Driven Workflows**: Visual specifications becoming executable code

### The New Development Stack

Traditional development workflows relied on human developers manually translating requirements into code. The agentic development stack introduces AI agents as first-class team members:

```text
┌─────────────────────────────────────────────────────────────┐
│  Traditional Development Stack                               │
└─────────────────────────────────────────────────────────────┘

Requirements → Design → Code → Test → Deploy
     ↓           ↓        ↓       ↓       ↓
   Human      Human    Human   Human   Human
  (PM/BA)  (Designer)  (Dev)   (QA)   (DevOps)

┌─────────────────────────────────────────────────────────────┐
│  Agentic Development Stack                                   │
└─────────────────────────────────────────────────────────────┘

Spec (SDD) → Design (Figma MCP) → Code (Agents) → Test (Auto) → Deploy (Auto)
     ↓            ↓                   ↓              ↓              ↓
   Human        Human            AI Agents      AI Agents      AI Agents
  (PM/BA)    (Designer)         (Copilot,       (Q Dev,        (Q Dev,
                                 Claude,         Selenium)      AWS)
                                 Q Dev)
```

---

## 2. Spec-Driven Development (SDD) Foundations

### The Core Problem: 'Vibe Coding' vs. 'Verifiable Intent'

The fundamental challenge in integrating AI agents into production software development is the **"specification problem"**.

Unstructured prompting, often called **"vibe coding"**, fails at scale because it forces the AI agent—which is exceptional at pattern completion but not "mind reading"—to guess at thousands of unstated requirements. This leads to inconsistent, unscalable, and incorrect results.

**Spec-Driven Development (SDD)** solves this by elevating specifications from static documentation into **executable, first-class artifacts**. The spec becomes the **"source of truth"** that provides explicit, machine-understandable guardrails, ensuring verifiable alignment between human intent and AI execution.

### Three SDD Implementation Frameworks

#### Implementation 1: Kiro & AWS AI-DLC

**Kiro** is an agentic IDE built on the Code OSS platform, purpose-built around three critical specification files:

1. **requirements.md**: Captures **what** needs to be built using EARS (Easy Approach to Requirements Syntax) format
2. **design.md**: Outlines the **technical architecture**, components, data models, and interfaces
3. **tasks.md**: Breaks the design into a **checklist of coding tasks**

**Example Kiro Workflow:**

```text
Phase 1: Inception (Requirements)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Developer: "Build a payment processing system"
Kiro Agent: Generates requirements.md

requirements.md:
- User Story: As a customer, I want to process credit card payments
- Acceptance Criteria (EARS format):
  * WHEN customer clicks "Pay", THEN payment form displays
  * WHEN payment succeeds, THEN order confirmation email sent
  * IF payment fails, THEN error message displayed

Phase 2: Construction (Design → Tasks)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Kiro Agent: Generates design.md from requirements.md

design.md:
- Architecture: Microservices (API Gateway → Payment Service → Database)
- Tech Stack: Node.js, Express, Stripe API, PostgreSQL
- Data Models: Order, Payment, Customer
- APIs: POST /api/payments, GET /api/payments/:id

Kiro Agent: Generates tasks.md from design.md

tasks.md:
☐ Task 1: Set up Express server
☐ Task 2: Integrate Stripe SDK
☐ Task 3: Create Payment model
☐ Task 4: Implement POST /api/payments endpoint
☐ Task 5: Add error handling
☐ Task 6: Write unit tests

Phase 3: Implementation (Agent Execution)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Kiro Agent: Executes tasks.md, writes code
Agent Hooks: Auto-update docs, run security scan on save
```

**Key Benefits:**

- ✅ Single Source of Truth - All specs version-controlled in Git
- ✅ Agent Long-Term Memory - Specs persist across sessions
- ✅ Automated Consistency - Agent hooks keep docs synchronized
- ✅ EARS Format - Structured requirements prevent ambiguity

#### Implementation 2: GitHub Spec Kit

**GitHub Spec Kit** is an open-source toolkit (CLI, templates, and prompts) designed to formalize the SDD process for any AI coding assistant. It implements a structured, five-phase workflow:

**Phase 1: `/speckit.constitution`** - Defines project governance principles

**Phase 2: `/speckit.specify`** - Defines the what and why

**Phase 3: `/speckit.plan`** - Defines the technical how

**Phase 4: `/speckit.tasks`** - Breaks plan into actionable checklist

**Phase 5: `/speckit.implement`** - Instructs agent to execute tasks

**Example GitHub Spec Kit Workflow:**

```bash
# Step 1: Establish project governance
/speckit.constitution

Output: constitution.md
---
# Project Constitution

## Development Philosophy
- Code must be readable and maintainable
- Security is non-negotiable
- Every feature must have tests

## Coding Standards
- TypeScript strict mode enabled
- ESLint + Prettier for formatting
- Functional programming preferred
---

# Step 2: Define what to build
/speckit.specify

Output: spec.md
---
# Feature Specification: User Authentication

## Business Requirements
- Users must be able to sign up with email/password
- Users must be able to log in and log out
- Passwords must be hashed (bcrypt)
---

# Step 3: Plan the technical implementation
/speckit.plan

Output: plan.md
---
# Implementation Plan: User Authentication

## Tech Stack
- Backend: Node.js + Express
- Database: PostgreSQL
- Auth: bcrypt + jsonwebtoken (JWT)
---
```

**Key Benefits:**

- ✅ AI-Agnostic - Works with any AI coding assistant
- ✅ Flexible - CLI commands adapt to any workflow
- ✅ Quality Assurance - Built-in validation commands
- ✅ Lightweight - Markdown files in Git, no proprietary tooling

#### Implementation 3: Agent OS

**Agent OS** is an open-source "operating system for spec-driven development" that provides a 3-Layer Context architecture:

**Layer 1: Standards/** - How your team builds

- Coding style guides, architecture patterns, security standards

**Layer 2: Product/** - Why you are building

- Product vision, user personas, roadmap

**Layer 3: Specs/** - What you are building next

- Feature requirements, technical design, task breakdown

**The 6-Phase Workflow:**

1. **plan-product** - Define product vision and roadmap
2. **shape-spec** - Rough out feature concept
3. **write-spec** - Detailed specification creation
4. **create-tasks** - Break spec into actionable tasks
5. **implement-tasks** - Agent executes tasks
6. **orchestrate-tasks** - Coordinate multi-agent collaboration

**Key Benefits:**

- ✅ Tool-Agnostic - Works with any AI agent
- ✅ Enterprise-Grade - Enforce custom standards and patterns
- ✅ Scalable - 3-layer context scales to complex organizations
- ✅ Multi-Agent Ready - Orchestrate multiple agents with shared context

### Comparative Analysis Table

| Framework | Core Philosophy | Key Artifacts | Ideal Use Case |
|-----------|-----------------|---------------|----------------|
| **Kiro** | Integrated Agentic IDE | requirements.md, design.md, tasks.md | Teams seeking all-in-one, highly structured IDE |
| **GitHub Spec Kit** | Flexible, AI-agnostic toolkit | constitution.md, spec.md, plan.md, tasks.md | Teams adding structure to existing AI tools |
| **Agent OS** | Tool-agnostic "Operating System" | Standards/, Product/, Specs/ | Mature enterprises enforcing custom standards |

### FSI Recommendation

For financial services institutions, the recommended approach is:

1. **Start with GitHub Spec Kit** for rapid adoption and proof-of-concept
2. **Evaluate Kiro** for teams seeking integrated IDE experience
3. **Graduate to Agent OS** for organization-wide standardization at scale

---

## 3. Model Context Protocol (MCP): Universal Integration

### The 'USB-C for AI'

MCP is an open standard that solves the critical **"fragmentation"** problem of AI models. Historically, connecting an AI agent to five different tools required five different custom, brittle API integrations.

**MCP**, introduced by **Anthropic in late 2024**, provides a **universal standard for AI-tool communication**, effectively acting as the **"USB-C of AI integration"**.

### MCP vs. Orchestration: A Critical Distinction

It's critical to distinguish **MCP** from **orchestration frameworks** like **LangChain**.

**LangChain** (and similar frameworks) acts as the **"brain organizer"**:

- Agent framework that defines workflow, reasoning, and logic chains
- Decides **when** to call a tool and **for what purpose**
- Handles intelligence layer (planning, reasoning, decision-making)

**MCP** acts as the **"universal translator"**:

- Standardized integration layer or protocol
- Not an agent framework, does not define logic
- Handles connection, authentication, error handling, and data exchange

```text
┌─────────────────────────────────────────────────────────────┐
│  The Agentic Architecture: Orchestration + MCP              │
└─────────────────────────────────────────────────────────────┘

Layer 3: Intelligence (Orchestration Framework)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LangChain, crewAI, AutoGen
- Defines workflow and reasoning logic
- Decides WHEN to call tools
- Handles multi-step planning

         ↕ (uses)

Layer 2: Integration (Model Context Protocol)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
MCP Servers (standardized adapters)
- Handles HOW to call tools
- Manages authentication and errors
- Provides standardized data exchange

         ↕ (connects to)

Layer 1: Tools & Data Sources
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PostgreSQL, GitHub, Jira, Salesforce, Slack, AWS
```

### Core MCP Architecture

The MCP architecture is a standardized client-server model:

**MCP Host**: The AI application (VS Code, Claude Desktop, Cursor, Windsurf)
**MCP Client**: Component instantiated by the Host, one-to-one with MCP server
**MCP Server**: Program that provides context via:

1. **Tools**: Functions the agent can call
2. **Resources**: Data sources the agent can read
3. **Prompts**: Pre-defined templates

```text
User
  ↕
┌─────────────────────────────────────────────┐
│  MCP Host (AI Application)                   │
│  e.g., VS Code, Claude Desktop               │
│                                              │
│  ┌─────────────────────────────────────┐   │
│  │  MCP Client 1                        │   │──→ MCP Server: PostgreSQL
│  └─────────────────────────────────────┘   │
│  ┌─────────────────────────────────────┐   │
│  │  MCP Client 2                        │   │──→ MCP Server: GitHub
│  └─────────────────────────────────────┘   │
│  ┌─────────────────────────────────────┐   │
│  │  MCP Client 3                        │   │──→ MCP Server: Figma
│  └─────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

### Strategic Implications

MCP adoption has profound implications for enterprise architecture:

#### 1. Commoditization of Integration

MCP commoditizes the integration layer. Complex custom code is now handled by the protocol.

**Before MCP**: 100+ lines of custom integration code per tool
**After MCP**: 5-10 lines of JSON configuration per tool

#### 2. Plug-and-Play Ecosystem

The MCP Marketplace provides pre-built servers for:

- AWS services (S3, DynamoDB, Lambda, CloudWatch)
- Development tools (GitHub, GitLab, Jira)
- Data platforms (PostgreSQL, MySQL, MongoDB, Snowflake)
- Communication (Slack, Microsoft Teams)
- Design (Figma, Miro)

#### 3. Vendor Independence

Organizations can switch between AI agents without rebuilding integrations. An MCP server works with any MCP-compatible host.

---

## 4. The Design Phase: Figma MCP

### Introduction: The 'Design MCP' as the Bridge

The SDD workflow begins with the **specification**, which for modern applications is increasingly **visual**. The **Figma MCP server** is the purpose-built tool that exposes Figma design files as **structured context** to AI agents.

**Key Insight:** Figma MCP functions as the **critical architectural bridge**, translating visual specifications into a format that code-generation agents can understand and implement.

### Connection and Access

An MCP client can connect to the Figma MCP server in two ways:

#### 1. Desktop MCP Server

**Characteristics:**

- Runs locally through Figma desktop app
- Enabled via Dev Mode
- Endpoint: `http://127.0.0.1:3845/mcp`

**Use Case:** Best for developers working locally with direct Figma access

#### 2. Remote MCP Server

**Characteristics:**

- Connects directly to Figma's global endpoint
- Endpoint: `https://mcp.figma.com/mcp`
- No local installation required

**Use Case:** Best for CI/CD pipelines, cloud-based development, team collaboration

### Accessing Design Nodes

Once connected, agents can access design nodes in two ways:

#### Selection-Based Access

Agent reads the user's current selection in the Figma desktop app.

**Workflow:**

```text
1. Designer selects a frame/component in Figma
2. Agent queries: "Generate code for the selected design"
3. MCP server returns structured data for selection
4. Agent generates code
```

#### Link-Based Access

Provide a Figma URL directly to the agent.

**Workflow:**

```text
Developer: "Generate code for https://figma.com/file/abc123/node-id=456"
           ↓
Agent queries MCP server with URL
           ↓
MCP returns structured design data
           ↓
Agent generates code
```

### Core Tools and Capabilities

#### 1. Generate Code from Selected Frames

Transform selected Figma frames into production code.

**Supported Outputs:**

- HTML/CSS
- React components
- Vue components
- React Native
- SwiftUI
- Flutter

**Example:**

```text
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

#### 2. Extract Design Context

Pull design tokens, components, and layout data directly from Figma.

**Extracted Information:**

- **Design Tokens**: Colors, typography, spacing, borders, shadows
- **Components**: Hierarchy, props, variants, states
- **Layout Data**: Auto-layout, responsive breakpoints, grid systems

**Use Case:**

```text
Scenario: Developer needs consistent design system implementation

Agent Action:
1. Extract all color tokens from Figma → Generate colors.ts
2. Extract typography scale → Generate typography.css
3. Extract button variants → Generate Button.tsx
4. Ensure new feature uses only extracted tokens
```

#### 3. Retrieve FigJam/Make Resources

Access early-stage ideas, user flows, or architecture maps from FigJam and Make files.

**FigJam Use Cases:**

- User journey maps → Generate user story specs
- Wireframes → Generate initial component structure
- Brainstorming boards → Extract feature requirements

**Workflow:**

```text
Product Manager creates user flow in FigJam
           ↓
Agent extracts flow using Figma MCP
           ↓
Agent generates:
  - User stories (requirements.md)
  - Acceptance criteria
  - Test scenarios
```

#### 4. Code Connect Integration

The **key to maintaining enterprise-grade UI consistency**. Code Connect ensures generated code reuses actual, existing components from your codebase.

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

Create mapping files that connect Figma components to actual code components:

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

**Benefits:**

- ✅ Design System Compliance - All generated code uses approved components
- ✅ Type Safety - Props match actual TypeScript interfaces
- ✅ Maintainability - Updates to design system automatically propagate
- ✅ Consistency - No visual drift between design and implementation

**Enterprise FSI Use Case:**

```text
Scenario: Bank has strict design system with 50+ regulated components

Code Connect ensures:
- All generated UIs use approved components
- Accessibility standards (WCAG 2.1 AA) automatically met
- Brand guidelines enforced
- Compliance requirements (required disclosures) included
```

### Best Practices

#### 1. Figma File Organization

**Clear Naming:**

```text
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

- Use consistent component variants
- Apply Auto Layout for responsive behavior
- Define constraints explicitly
- Use component properties for dynamic content

**Design Tokens:**

- Create Figma variables for colors, spacing, typography
- Organize variables into collections
- Use semantic naming (e.g., "color-primary-500" not "color-blue")

#### 2. Write Effective Prompts

**Specific Instructions:**

```text
✅ Good Prompt:
"Generate a React component for the selected card design. Use Tailwind CSS 
for styling, implement responsive breakpoints at 768px and 1024px, ensure 
WCAG 2.1 AA compliance, and use our Button component from the design system 
for the CTA."

❌ Vague Prompt:
"Make this into code"
```

**Context Provision:**

```text
"This is a transaction history card for our banking app. It should:
- Display transaction amount, merchant, date, and category
- Support both debit (red) and credit (green) transactions
- Be clickable to show transaction details
- Work on mobile (375px) to desktop (1440px)
- Meet PCI DSS requirements (no sensitive data in DOM attributes)"
```

---

## 5. The Execution Phase: Agentic Code Generation

### From Tests to Implementation

With tests generated from design, AI agents now have:

1. Visual specification (Figma MCP)
2. Functional specification (BDD scenarios)
3. Test harness (Executable tests)

The agent's job: **Write code that passes all tests**.

### Test-Driven Agentic Development (TDAD)

**Workflow:**

```text
┌──────────────────────────────────────────────────────────┐
│  1. Agent reads Figma design via MCP                     │
│     - Extracts components, layout, styling               │
└────────────────┬─────────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────────┐
│  2. Agent reads generated test scenarios                 │
│     - Understands expected behavior                      │
│     - Identifies edge cases                              │
└────────────────┬─────────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────────┐
│  3. Agent generates initial implementation               │
│     - Creates component structure                        │
│     - Implements styling from Figma                      │
│     - Uses Code Connect mappings                         │
└────────────────┬─────────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────────┐
│  4. Agent runs tests                                     │
│     - Executes Playwright/Jest tests                     │
│     - Captures failures                                  │
└────────────────┬─────────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────────┐
│  5. Agent iterates until all tests pass                  │
│     - Analyzes test failures                             │
│     - Refines implementation                             │
│     - Re-runs tests                                      │
└────────────────┬─────────────────────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────────────────────┐
│  6. Agent creates pull request                           │
│     - Generated code                                     │
│     - Test results                                       │
│     - Design-to-code traceability                        │
└──────────────────────────────────────────────────────────┘
```

### Agent Execution Tools

**Code Generation:**

- Claude Agent SDK - Core orchestration
- Claude Code - IDE-integrated agent
- GitHub Copilot - Alternative agent

**Testing Frameworks:**

- Playwright - End-to-end testing
- Jest - Unit testing
- Cypress - Component testing
- Storybook - Visual regression testing

**Quality Assurance:**

- ESLint - Code quality
- Prettier - Code formatting
- TypeScript - Type checking
- Lighthouse - Performance and accessibility

---

## 6. Agent Comparison & Selection

### The Multi-Agent Development Team

These agents are not interchangeable—they represent specialized roles:

#### GitHub Copilot: The 'In-Flow' Pair Programmer

**Core Capability:** Deeply integrated IDE assistance for in-the-flow coding

**Strengths:**

- Line-level code completion
- Boilerplate generation
- Multi-model access (Pro+ plan: Claude Opus, GPT-5, Gemini 2.5 Pro)
- Superior context engineering from IDE integration

**Limitations:**

- Claude Opus only available in "Ask Mode", not "Agent Mode"
- Best for line-level tasks, not complex multi-file operations

**Pricing:** $10-$39/month
**Best For:** Daily coding, every developer, inner-loop development

#### Claude Code: The 'Deep-Context' Architect

**Core Capability:** Massive context window, terminal-first conversational agent

**Superpower:**

- Can ingest entire project (50,000+ lines)
- Enables deep codebase understanding
- Complex multi-file refactoring
- System-wide debugging

**Use Cases:**

```text
- Analyze authentication system across entire codebase
- Refactor API to consistent error handling (200+ endpoints)
- Find memory leak in complex application
- Create persistent "TypeScript Police" agent
```

**Pricing:** $20-$200/month
**Best For:** Senior engineers, complex refactoring, architectural work

#### AWS Q Developer: The 'Enterprise SDLC' Agent

**Core Capability:** End-to-end SDLC automation across AWS ecosystem

**Key Agentic Commands:**

- `/dev` - Feature implementation (API, Lambda, infrastructure)
- `/test` - Automated test generation
- `/review` - Workspace-wide code & security review
- `/upgrade` - Legacy application modernization
- `/transform` - Database migration (Oracle → PostgreSQL)

**AWS Ecosystem Moat:**

- Trained on 17 years of AWS experience
- Agentic root-cause analysis from CloudWatch logs
- Deep integration with AWS console and CLI

**Example RCA Workflow:**

```text
Production alert: "API latency spike"
Engineer invokes Q: "Analyze latency spike at 14:30 UTC"

Q Developer:
- Queries CloudWatch Logs
- Analyzes Lambda traces
- Checks DynamoDB throttling
- Reviews X-Ray traces

Result: Root cause identified, fixes proposed, implementation offered
```

**Pricing:** $19/user/month
**Best For:** AWS development, DevOps/SRE, full SDLC automation

### Agent Comparison Table

| Agent | Primary Interface | Core Strength | Key Differentiator | Pricing |
|-------|------------------|---------------|-------------------|---------|
| **GitHub Copilot** | IDE | In-Flow Pair Programming | Deep IDE integration, multi-model | $10-$39/mo |
| **Claude Code** | Terminal/Chat | Deep-Context Refactoring | Massive context window (entire project) | $20-$200/mo |
| **AWS Q Developer** | IDE & AWS Console | Enterprise SDLC Automation | 17 years AWS training, CloudWatch RCA | $19/user/mo |

### Multi-Agent Deployment Strategy

**Recommended Portfolio for FSI (50 developers):**

```text
Investment Breakdown:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

GitHub Copilot Pro: 50 × $10/month = $500/month
├─ Every developer
├─ Daily coding, boilerplate, quick tasks
└─ ROI: 20-30% productivity increase

Claude Code Max: 10 × $150/month = $1,500/month
├─ Senior engineers, architects
├─ Complex refactoring, system design
└─ ROI: Reduce refactoring weeks → days

AWS Q Developer: 50 × $19/month = $950/month
├─ All developers + DevOps
├─ Test generation, security review, RCA
└─ ROI: Automated testing, faster incidents

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total: ~$3,000/month for 50 developers
Per-Developer: $60/month

ROI: 20-30% productivity = 10-15 additional developers
Traditional hiring: $150,000-$225,000/month

NET ROI: ~50x return on investment
```

---

## 7. Best Practices & Patterns

### Specification Quality

**Good Specifications:**

- Use EARS format for requirements (WHEN... THEN...)
- Define acceptance criteria explicitly
- Include non-functional requirements
- Provide context and constraints

### Design System Governance

**Code Connect Enforcement:**

- Map all design components to code components
- Enforce through CI/CD checks
- Regular design-code sync audits
- Version control for mappings

### Agent Workflow Optimization

**Context Management:**

- Keep specifications updated in Git
- Use agent hooks for automatic doc updates
- Maintain design token libraries
- Document architecture decisions

### Testing Strategy

**Comprehensive Coverage:**

- Generate tests from specifications
- Visual regression with AI-powered validation
- Accessibility testing (WCAG 2.1 AA)
- Performance testing integration

---

## 8. FSI Use Cases

### Banking Application Development

**Scenario:** Customer transaction dashboard

**Workflow:**

1. Product manager defines requirements in FigJam
2. Designer creates mockups in Figma
3. Agent extracts user flows → generates requirements.md
4. Agent reads Figma design → generates React components using Code Connect
5. Agent generates BDD test scenarios
6. Agent implements code to pass all tests
7. Agent creates pull request with traceability

**Compliance Benefits:**

- Audit trail from requirements to code
- Design system compliance enforced
- Accessibility standards met automatically
- Security review integrated

### Legacy Modernization

**Scenario:** Upgrade Java 8 → Java 17 banking application

**Q Developer Workflow:**

```text
/upgrade Upgrade Java 8 application to Java 17 with AWS best practices

Q Developer:
1. Updates Java version
2. Replaces deprecated APIs
3. Updates AWS SDK (v1 → v2)
4. Modernizes Lambda runtime
5. Updates IAM policies
6. Migrates logging to CloudWatch

Result: 39 automatic fixes, 8 manual reviews required
```

---

## Related Modules

- **[Product Management](../product-management/README.md)** - Design thinking and product strategy
- **[Lens 4: Quality & Testing](../lens-4-quality-testing/README.md)** - Comprehensive testing strategies
- **[Lens 5: AI, Inference & Agentic Automation](../lens-5-ai-inference-automation/README.md)** - Advanced AI orchestration

---

## Additional Resources

- **[Complete Workflow Guide](../../AGENTIC_WORKFLOW_PRACTICE.md)** - Full platform documentation
- **[Banking Use Case](../../AGENTIC_BANK_FSI.md)** - Financial services implementation
- **[Main README](../../README.md)** - Platform overview

---

**Status**: Full content migration complete

**Last Updated**: 2025-01-17

**Maintainers**: Platform Architecture Team
