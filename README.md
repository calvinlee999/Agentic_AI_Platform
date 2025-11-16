# Agentic AI Platform

## Implementation Blueprints for Claude on Enterprise Cloud Platforms

> *Transforming Financial Services from AI Inference to AI Agency*

---

## Table of Contents

- [Executive Summary](#executive-summary)
- [The Three Pillars](#the-three-pillars)
- [Key Executive Findings](#key-executive-findings)
- [Use Cases: From Inference to Agency](#use-cases-from-inference-to-agency)
- [Multi-Platform Implementation](#multi-platform-implementation)
- [Agentic Spec-Driven Development (SDD)](#agentic-spec-driven-development-sdd)
- [Strategic Recommendations](#strategic-recommendations)
- [Implementation Roadmap](#implementation-roadmap)
- [Getting Started](#getting-started)

---

## Executive Summary

### The New Paradigm: From AI Inference to AI Agency

For the past decade, artificial intelligence (AI) in the financial services industry (FSI) has been synonymous with machine learning (ML) inference—passive models trained on vast datasets to make predictions. While valuable, these applications were fundamentally **reactive**.

The introduction of high-capability generative AI (GenAI) models, particularly **Anthropic's Claude 3.x and Sonnet series**, represents a profound paradigm shift: the move from **passive prediction to active participation**.

### The Frontier Firm

We are transitioning from AI as a discrete analytical tool to AI as an integrated, **agentic orchestrator** of complex business processes. This new model is best encapsulated by the concept of the **"Frontier Firm"**—an enterprise that fundamentally re-architects itself around intelligence.

In this model:
- AI agents are treated as **"digital colleagues"**
- Human employees become **"agent bosses"** who manage and delegate tasks
- Systems are **AI-operated but remain human-led**

This is the **era of agentic AI**: a blend of machine intelligence and human judgment.

---

## The Three Pillars

This repository provides a comprehensive architectural and strategic blueprint for financial institutions seeking to become "Frontier Firms" by leveraging Claude and its agentic capabilities. The analysis is structured around three foundational pillars:

### 1. **The Use Cases (The "Why")**

Three high-value banking applications progressing in complexity:

- **Advanced AI Inference** - Risk & Fraud Detection
- **Agentic Business Automation** - Customer Onboarding & Internal Operations
- **Agentic Product Development** - Using AI to Build AI

### 2. **The Platforms (The "Where")**

Deep-dive analysis of five primary enterprise ecosystems:

- **Amazon Web Services (AWS)** - Bedrock, Amazon Q, Kiro
- **Microsoft Azure** - AI Foundry, Copilot, Model Context Protocol (MCP)
- **Databricks** - Mosaic AI, Agent Bricks, LSEG Partnership
- **Snowflake** - Intelligence, Cortex Analyst
- **NVIDIA AI Foundry** - Custom Model Development

### 3. **The Methodology (The "How")**

Investigation into **Agentic Spec-Driven Development (SDD)** and critical tools:

- AWS Kiro
- GitHub Spec Kit
- Agent OS
- Claude Agent SDK

---

## Key Executive Findings

### 1. Platform Divergence Defines Strategy

The choice of a primary AI platform is a **profound strategic commitment** that extends far beyond model access. Each platform embodies a distinct architectural philosophy:

| Platform | Philosophy | Best For |
|----------|-----------|----------|
| **AWS** | Deep vertical integration | Broad, integrated applications |
| **Azure** | Horizontal, protocol-driven ecosystem (MCP) | M365/Internal productivity |
| **Databricks** | Data-centric, lakehouse-native "on-data" | Real-time analytics & risk |
| **Snowflake** | Warehouse-centric, SQL-native "in-data" | BI & data democratization |

### 2. The "Tool" is the Differentiator, Not the Model

The most critical component in agentic architecture is **not the LLM itself** (which is becoming commoditized), but the **"plumbing"** that connects the model's reasoning to:

- The bank's proprietary data
- Secure systems
- Business logic

**Critical Enablers:**
- **Claude Agent SDK** - Provides standardized, secure, extensible tool-calling capabilities
- **Model Context Protocol (MCP)** - Microsoft's open standard for tool integration

### 3. Development as an Auditable Specification

For highly regulated industries like banking, **Agentic Spec-Driven Development (SDD)** is non-negotiable.

Traditional AI development has been criticized as "vibe-driven" and opaque. SDD transforms this by:

- Requiring **human-in-the-loop approval** of structured, version-controlled specification files
- Creating a **persistent, transparent, end-to-end audit trail**
- Making the entire SDLC (Software Development Lifecycle) **compliant by design**

**Key Artifacts:**
- `requirements.md` - What to build
- `design.md` - How to build it  
- `tasks.md` - Step-by-step implementation plan

---

## Use Cases: From Inference to Agency

The deployment of Claude models in banking spans a **maturity curve**, beginning with enhancement of existing ML tasks and progressing to fully autonomous agentic systems.

### Use Case 1: Advanced AI Inference (Risk & Fraud)

**The Classic Problem:**  
Traditional ML models analyze structured transaction data to produce risk scores or fraud flags.

**Claude's Differentiator:**  
The ability to **reason on unstructured data** with large context windows.

**Blueprint: The Analyst Co-pilot for Risk**

```
┌─────────────────────────────────────────────────────────┐
│  Alert: Anomalous high-value transaction detected      │
└──────────────────┬──────────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────────┐
│  Claude-Powered Agent (Amazon Q)                        │
│  • Pulls multi-modal data (structured + unstructured)  │
│  • Analyzes complete context                           │
│  • Cross-references fraud patterns                     │
│  • Generates plain-English summary with citations      │
└──────────────────┬──────────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────────┐
│  Human Analyst receives recommendation + context        │
└─────────────────────────────────────────────────────────┘
```

**Key Value:**
- Synthesizes structured transaction history, unstructured chat logs, PDF documents, and external news
- Combats "model drift" by identifying new fraud patterns
- Provides citations and clear recommendations

### Use Case 2: Agentic Business Automation

**Beyond Inference:**  
Multi-step, stateful, orchestrated workflows where the agent **executes** business processes by calling a series of tools.

**Example Blueprint: Secure Customer Onboarding**

```
Platform: Claude 3.5 Sonnet on Amazon Bedrock + LangChain Agent

Workflow (Tool-Calling):
1. Email Validation Tool → API Gateway → DynamoDB
2. ID Verification Tool → Amazon Textract  
3. Selfie Verification Tool → Amazon Rekognition (facial comparison)
4. Save Data Tool → DynamoDB (create new account)
```

**Key Value:**
- Reduces onboarding time from **days to minutes**
- Automates complex, high-friction, compliance-heavy processes
- Fully auditable workflow

**Example Blueprint: Internal Analyst Agent**

Personal agents (Microsoft Copilot) that act as "virtual data scientists":
- Transform raw internal data into forecasts
- Generate customer behavior visualizations
- Create automated reports for risk and compliance
- Empower employees as "agent bosses"

### Use Case 3: Agentic Product Development & Testing

**The Meta-Use Case: Using AI to Build AI**

AI agents transition from **users** of bank systems to **builders and maintainers** of those systems.

**The Problem:**  
Bank technology departments constrained by technical debt and overhead. National Australia Bank (NAB) case study: 3,000 developers spending only **2.7 days per week actively coding**.

**The Solution:**  
Amazon Q Developer to "automate repetitive coding tasks, update older code, and conduct compliance checks."

**The Methodology: SDD as a Compliance Tool**

```
Human-Approved Spec (requirements.md, design.md)
          ↓
   AI Agent (Claude Code / Amazon Q / Copilot)
          ↓
Generated Code + Tests + Documentation
          ↓
Transparent, Auditable Trail (version-controlled)
```

**Key Value for Banking:**
- **Speed:** 55% faster task completion
- **Developer Experience:** Better flow state maintenance
- **Compliance & Auditability:** Clear trail from requirement → design → implementation

---

## Multi-Platform Implementation

### Platform Comparison Matrix

| Feature | AWS | Azure | Databricks | Snowflake |
|---------|-----|-------|------------|-----------|
| **Native Claude 3.x Access** | ✅ First-Party | ⚠️ Catalog/BYO | ⚠️ BYO | ✅ Managed |
| **Primary Agentic Framework** | Amazon Q / Bedrock Agents | Copilot / Agent Service | Agent Bricks | Cortex Agents |
| **Data Proximity** | PaaS (Near-Data) | PaaS (Near-Data) | Lakehouse-Native (On-Data) | Warehouse-Native (In-Data) |
| **Key FSI Differentiator** | AWS Kiro (SDD IDE) | MCP (Standardized Tooling) | LSEG Partnership | Cortex Analyst (Text-to-SQL) |
| **Primary Use Case Fit** | Broad, Integrated Apps | M365/Internal Productivity | Real-Time Analytics/Risk | BI & Data Democratization |

### AWS: The Integrated Claude Ecosystem

**Core Services:**
- **Amazon Bedrock** - Secure, private, first-party API access to Claude 3.5 Sonnet
- **Bedrock Agents** - Fully managed orchestration with built-in Computer, Text Editor, and Bash tools
- **Amazon Q Business** - RAG-powered agent for enterprise knowledge
- **Amazon Q Developer** - IDE-integrated agent for building, testing, and modernizing code

**Implementation Pattern:**
```
Amazon Bedrock (Claude) 
  → Bedrock Agents (Orchestration) 
  → AWS Lambda (Tools) 
  → AWS Services (Textract, Rekognition, DynamoDB)
```

**SDD Environment:**  
**AWS Kiro** - Purpose-built "agentic AI IDE" for spec-driven development

### Azure: The Copilot-Centric, Open-Model Ecosystem

**Core Services:**
- **Azure AI Foundry** - Unified PaaS with 11,000+ models catalog (GPT, Llama, Mistral, DeepSeek)
- **AI Foundry Agent Service** - Managed runtime for orchestration, governance, and RBAC

**The Key Enabler: Model Context Protocol (MCP)**

The most strategically important component—an **open, standardized protocol** for secure tool discovery and interaction.

```
Front-end (Copilot) 
  → Agent (FastAPI app) 
  → MCP Client 
  → MCP Server (Azure MCP Server - 35+ Azure services)
  → Azure Resource Manager (with RBAC)
```

**Key Value:**
- Natural language interaction with entire Azure subscription
- "Show me all my resource groups"
- "Query my log analytics workspace for errors"

**Agentic Layer:**
- **Microsoft Copilot** (M365) - User-facing agents
- **Copilot Studio** - Low-code customization for internal agents

**Multi-Cloud Architecture:**  
Azure applications can use Claude Agent SDK to securely call Claude models on Amazon Bedrock (private, non-public-internet)

### Clarifying "AI Foundry": Azure vs. NVIDIA

⚠️ **Critical Distinction:**

| | Azure AI Foundry | NVIDIA AI Foundry |
|---|------------------|-------------------|
| **Type** | PaaS | Custom Model Service |
| **Purpose** | Deploy & orchestrate pre-built models | Build new, custom, proprietary models |
| **User** | Bank's IT department | Bank's quantitative trading desk |
| **Output** | Deployed agent using catalog model | Custom-trained model (NVIDIA NIM) |
| **Example** | Compliance reporting agent using GPT-5 | Proprietary "Trading-Llama-v1" model |

**Recommendation:** These are **complementary**, not competitive.

### Databricks: The Data-Centric AI Platform

**Core Strategy:**  
**Data proximity** - AI platform lives **on the data** within Databricks Lakehouse (eliminates costly data movement).

**Agentic Layer: Agent Bricks**

Three primary agent types:
1. **Information Extraction Agent** - Converts unstructured documents to structured lakehouse tables
2. **Knowledge Assistant Agent** - RAG agent for enterprise data queries
3. **Multi-Agent Supervisor** - Coordinates multiple specialized agents

**Strategic Enabler: LSEG Partnership**

LSEG (London Stock Exchange Group) datasets natively available via Delta Sharing:
- Lipper Fund Data
- Cross Asset Analytics
- Pricing data and tick history

**Implementation Blueprint:**
```
Agent Bricks 
  → Unified Query (LSEG market data + bank's proprietary data)
  → No data silos or ETL pipelines
```

**Resulting Use Cases:**
- Real-time investment analytics
- AI-driven backtesting and portfolio optimization
- Trade analytics with predictive forecasting
- Unified risk management (market, credit, counterparty)

### Snowflake: The In-Database AI Engine

**Core Strategy:**  
"In-database" AI for **democratizing data access** for business users, not just data scientists.

**Core Service: Snowflake Intelligence**

Powered by **Snowflake Cortex AI** suite.

**Implementation Pattern:**

```
Business User: "What coverages does this insurance company provide?"
          ↓
Snowflake Intelligence (routes to appropriate agent)
          ↓
Cortex Analyst (text-to-SQL agent)
          ↓
Dynamically generates SQL query
          ↓
Executes securely within governance framework
          ↓
Returns natural language answer
```

**Claude Integration:**
- Fully managed access to Claude 4.0 and Claude 4.5 Sonnet
- Example: Dataiku agent on Snowflake using Claude 4.5 Sonnet with three tools:
  1. Dataset lookup in Snowflake
  2. Custom ML model (default risk predictor)
  3. Cortex Search (RAG for policy documents)

**LLM-as-a-Judge:**  
Uses Claude 3 to automatically evaluate SQL quality generated by Cortex Analyst.

---

## Agentic Spec-Driven Development (SDD)

### The Core Philosophy: From "Vibe" to "Spec"

**The Problem:**  
"Vibe coding" or "prompting in circles"—giving AI vague, unstructured prompts—fails at scale because:
- LLMs are "exceptional at pattern completion, but not at mind reading"
- Vague requests force the model to guess at thousands of unstated requirements
- Results are inconsistent, unscalable, and often incorrect

**The Solution:**  
**Agentic Spec-Driven Development (SDD)** introduces a formal, multi-step process with **human-in-the-loop** as architect and reviewer.

### Key Principles

1. **Specification as Single Source of Truth**  
   - Human-created and approved
   - Version-controlled (requirements.md, design.md, tasks.md)
   - Acts as agent's long-term memory

2. **Business Value for FSI**
   - **Productivity:** 55% faster task completion
   - **Developer Experience:** Better flow state
   - **Compliance & Auditability:** Transparent, end-to-end audit trail

### SDD Tooling Comparison

| Methodology | AWS Kiro | GitHub Spec Kit | Agent OS |
|-------------|----------|-----------------|----------|
| **Type** | Integrated Product (IDE) | Open-Source Toolkit (CLI) | Agnostic Framework |
| **Core Philosophy** | Spec as single source of truth | Structured multi-step refinement | 3-Layer Context |
| **Key Artifacts** | requirements.md, design.md, tasks.md | constitution.md, specify.md, plan.md | Standards/, Product/, Specs/ |
| **Workflow** | 3-phase spec generation | 5-step CLI commands | 6-phase workflow |
| **Best For** | AWS-native teams | GitHub-native teams | Multi-cloud, mature teams |

### AWS Kiro: The Integrated IDE Approach

**Features:**
- Supports Claude Sonnet 4.0 and 3.7
- Transforms high-level prompt → formal spec → trackable tasks

**Three Critical Specification Files:**

```
requirements.md
  ├─ What needs to be built
  ├─ User stories
  └─ Acceptance criteria (EARS format)

design.md
  ├─ Technical architecture
  ├─ Components
  ├─ Data models
  └─ Interfaces

tasks.md
  └─ Step-by-step implementation checklist
```

**Case Study:**  
Kiro team "used the tool to build the tool"—turned multi-week feature into a **two-day task**.

### GitHub Spec Kit: The Open-Source Toolkit Approach

**Philosophy:**  
CLI-based toolkit for structured, multi-step refinement.

**Core Workflow (Slash Commands):**

```bash
/speckit.constitution  # Define project principles & guidelines
/speckit.specify       # Define what to build (requirements)
/speckit.plan          # Define how to build (tech stack, architecture)
/speckit.tasks         # Generate actionable task list
/speckit.implement     # Execute all tasks and build feature
```

### Agent OS: The Tool-Agnostic Framework Approach

**Philosophy:**  
"Operating system for spec-driven development"—works with any AI coding tool (Claude Code, Cursor, Gemini, etc.)

**3-Layer Context System:**

```
1. Standards/     ← How your team builds (coding standards, patterns)
2. Product/       ← Why you are building (mission, vision, roadmap)
3. Specs/         ← What you are building next (feature specifications)
```

**Six-Phase Workflow:**
1. plan-product
2. shape-spec
3. write-spec
4. create-tasks
5. implement-tasks
6. orchestrate-tasks

**Key Advantage:**  
Most flexible for mature enterprises—explicitly captures the bank's unique coding standards as executable specifications.

### The Foundational SDK: Claude Agent SDK

**Purpose:**  
The "plumbing" that connects Claude models to the real world.

**Key Features:**

- **Multi-Language SDKs:** Python (data science) and TypeScript (web/Node.js)
- **Built-in Tooling:** File operations, code execution, web search
- **Extensibility:** Connection to external tools via Model Context Protocol (MCP)
- **Permissions:** Fine-grained capability controls (allow/deny, policy modes)
- **Context Management:** Automatic compaction to prevent context overflow
- **Production Essentials:** Error handling, session management, monitoring

**Critical Architectural Insight: Multi-Cloud Authentication**

```bash
# Amazon Bedrock
CLAUDE_CODE_USE_BEDROCK=1

# Google Vertex AI
CLAUDE_CODE_USE_VERTEX=1
```

**Why This Matters:**  
Enables secure, multi-cloud architecture where:
- Application deployed in Azure virtual network
- Makes private, secure call to Claude 3.5 Sonnet in private Amazon Bedrock VPC
- Solves data residency, privacy, and security concerns

---

## Strategic Recommendations

### 1. Platform Selection is Driven by Your "Center of Gravity"

Choose your AI platform as an extension of your existing data and application strategy:

#### ☁️ AWS-Native Shop
- **Adopt:** Vertically integrated stack
- **Use:** Amazon Bedrock (private Claude), Amazon Q (pre-built agents), AWS Kiro (SDD IDE)

#### 🔷 Azure-Native (M365) Shop  
- **Focus:** Internal productivity and integration
- **Use:** Copilot + MCP ecosystem, Azure AI Foundry (orchestration/governance)
- **Model Strategy:** Treat models as plug-and-play commodities (including Claude from Bedrock)

#### 🏗️ Databricks Lakehouse-Centric
- **Differentiator:** LSEG partnership
- **Use:** Agent Bricks for data-native agents
- **Applications:** Real-time risk, trade, investment analytics (LSEG + enterprise data in-situ)

#### ❄️ BI & Business User Democratization
- **Focus:** Fastest path to value
- **Use:** Snowflake Intelligence + Cortex Analyst (text-to-SQL)
- **Value:** Empower non-technical users to query governed data with natural language

### 2. Standardize Your "Tools" Before Your "Agents"

**Key Insight:**  
Long-term value is in **tools**, not the LLM. The model is interchangeable; your proprietary business logic is not.

**Action:**  
Launch internal, **API-first initiative**:

```
Define core banking functions as secure, stateless, well-documented APIs:
  ├─ CheckKYC
  ├─ RunCreditRiskModel
  ├─ GetPortfolioHoldings
  └─ VerifyIdentity
```

**Benefits:**
- APIs become "tools" that any agent (on any platform) can call
- Commoditizes the LLM
- Prevents vendor lock-in

### 3. Adopt Spec-Driven Development (SDD) for Compliance

**Mandate:**  
Do not allow "vibe coding" or unstructured AI development.

**Action:**  
Mandate SDD methodology for all AI-assisted development.

**Recommended Starting Point:**  
**Agent OS framework** (most flexible and tool-agnostic)

**Implementation:**
```
Use Standards/ layer to codify:
  ├─ Bank's coding standards
  ├─ Security requirements
  └─ Compliance rules
```

**Result:**  
Executable "constitution" that all AI agents must follow → fully auditable and compliant-by-design SDLC

---

## Implementation Roadmap

### Phase 1: Govern (Months 0-6)
**Focus:** Build the "Tools"

**Actions:**
- Internal API-first initiative
- Expose core banking functions as secure, well-documented APIs
- Establish private, secure connection to Claude 3.5 Sonnet on Amazon Bedrock

**Deliverables:**
- API documentation
- Security & governance framework
- Private Claude endpoint

---

### Phase 2: Automate (Months 6-12)
**Focus:** Demonstrate value with contained business automation (Use Case 2)

**Actions:**
- Build one internal-facing agent (e.g., compliance report summarization)
- OR build one external-facing agent (e.g., customer onboarding workflow)
- Use Claude Agent SDK on Bedrock to orchestrate Phase 1 "tools"

**Deliverables:**
- Production agent
- Metrics (time savings, accuracy, user satisfaction)
- Lessons learned documentation

---

### Phase 3: Develop (Months 12-18)
**Focus:** Scale developer productivity and codify compliance (Use Case 3)

**Actions:**
- Roll out chosen SDD framework (Agent OS) to internal development teams
- Target legacy code modernization or test generation as first objectives

**Deliverables:**
- SDD methodology documentation
- Developer training program
- First AI-assisted development projects

---

### Phase 4: Revolutionize (Months 18+)
**Focus:** Build next-generation, data-native FSI applications (Use Case 1)

**Actions:**
- With agent-built applications, standardized tools, and unified data platforms (Databricks/Snowflake)
- Build next-generation, real-time risk and fraud models
- Fully integrated, auditable, and data-native

**Deliverables:**
- Advanced AI inference systems
- Competitive differentiation
- Measurable business outcomes

---

## Getting Started

### Prerequisites

- Access to one or more enterprise cloud platforms (AWS, Azure, Databricks, Snowflake)
- Claude API access (via Anthropic, Amazon Bedrock, or platform-specific integration)
- Internal APIs for core banking functions (or roadmap to build them)

### Quick Start

1. **Assess Your Current State**
   - Identify your "center of gravity" (AWS, Azure, Databricks, or Snowflake)
   - Inventory existing ML models and applications
   - Evaluate data governance and security posture

2. **Choose Your Platform Strategy**
   - Review [Multi-Platform Implementation](#multi-platform-implementation)
   - Select primary platform based on recommendations
   - Plan for multi-cloud if needed (e.g., Azure + Bedrock)

3. **Start with Phase 1: Govern**
   - Define 3-5 core banking APIs
   - Establish secure Claude endpoint
   - Create governance framework

4. **Pilot Use Case 2: Automate**
   - Select high-value, contained workflow
   - Build proof-of-concept agent
   - Measure and iterate

### Resources

- **AWS:** [Amazon Bedrock Documentation](https://aws.amazon.com/bedrock/)
- **Azure:** [Azure AI Foundry](https://azure.microsoft.com/en-us/products/ai-studio/)
- **Databricks:** [Mosaic AI Documentation](https://www.databricks.com/product/machine-learning)
- **Snowflake:** [Snowflake Intelligence](https://www.snowflake.com/en/data-cloud/cortex/)
- **Anthropic:** [Claude Documentation](https://docs.anthropic.com/)

---

## Contributing

We welcome contributions to improve this implementation blueprint. Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- Anthropic for Claude models and Agent SDK
- AWS, Microsoft, Databricks, and Snowflake for enterprise AI platforms
- The financial services community for use case validation and feedback

---

**Contact:** For questions or collaboration opportunities, please open an issue or reach out to the maintainers.

---

*Last Updated: November 16, 2025*
