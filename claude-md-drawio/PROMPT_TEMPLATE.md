# Project Analysis & Diagram Generation Prompt

---

## Copy-Ready Prompt (Single Copy-Paste)

> **Instructions:** Copy the entire block below, fill in the 4 parameters at the top, then paste to Claude.

```
## Project Analysis & Diagram Generation

### Parameters (FILL THESE FIRST):
- PROJECT_PATH: _______________
- OUTPUT_PATH: _______________
- ANALYSIS_TEMPLATES: _______________
- DIAGRAM_STANDARDS: _______________

### Standards & Templates
- Analysis Templates: [ANALYSIS_TEMPLATES]
- Diagram Standards: [DIAGRAM_STANDARDS]

### Project
Path: [PROJECT_PATH]

### Output Location
Save all outputs to: [OUTPUT_PATH]

### Important: Skip Irrelevant Sections
Skip any analysis/diagram type that doesn't apply to this project:
- No database → Skip ERD
- No cloud/IaC → Skip Infrastructure
- No complex workflows → Skip Flowchart/State Machine
- Simple app → Skip DDD, Security Analysis
Do not create empty files.

---

# STEP 1: Project Analysis

Generate structured markdown analysis files for applicable sections only.

## 1.1 Architecture Analysis
- Application overview (name, type, language, framework)
- Architecture pattern (MVC, Clean, Hexagonal, Layered)
- Entry points (API, UI, CLI, jobs)
- Components & modules with dependencies
- External integrations, cross-cutting concerns
→ Output: [OUTPUT_PATH]/analysis/architecture-analysis.md

## 1.2 Data Models / ERD Analysis (if database exists)
- Database type(s) and ORM
- Entities/tables with columns, types, constraints
- Relationships (1:1, 1:N, N:M), indexes, enums
→ Output: [OUTPUT_PATH]/analysis/data-models-analysis.md

## 1.3 Data Flow Analysis
- User input flows, API request flows
- Background job flows, event/message flows
- Data transformation points
→ Output: [OUTPUT_PATH]/analysis/data-flow-analysis.md

## 1.4 Flowchart / Process Analysis (if workflows exist)
- Business processes and decision points
- User journeys, approval workflows
→ Output: [OUTPUT_PATH]/analysis/flowchart-analysis.md

## 1.5 Sequence Diagram Analysis (if complex interactions)
- API interaction sequences
- Service-to-service flows, async message flows
→ Output: [OUTPUT_PATH]/analysis/sequence-analysis.md

## 1.6 Use Case Analysis (if documenting requirements)
- Actors, use cases with flows
- Include/extend relationships
→ Output: [OUTPUT_PATH]/analysis/use-case-analysis.md

## 1.7 State Machine Analysis (if stateful entities)
- Entities with states, transitions, triggers
→ Output: [OUTPUT_PATH]/analysis/state-machine-analysis.md

## 1.8 Infrastructure Analysis (if cloud/IaC exists)
- Cloud provider(s), compute, networking
- Data storage, security, monitoring, CI/CD
→ Output: [OUTPUT_PATH]/analysis/infrastructure-analysis.md

## 1.9 C4 Model Analysis
- Level 1: Context (system, actors, external systems)
- Level 2: Containers (deployable units)
- Level 3: Components (per container)
→ Output: [OUTPUT_PATH]/analysis/c4-model-analysis.md

## 1.10 DDD Analysis (if DDD patterns used)
- Domain/subdomains, bounded contexts
- Aggregates, domain events, services
→ Output: [OUTPUT_PATH]/analysis/ddd-analysis.md

## 1.11 Security Analysis (if security review needed)
- Auth methods, security boundaries
- Data protection, threat model
→ Output: [OUTPUT_PATH]/analysis/security-analysis.md

## 1.12 API Contract Analysis (if APIs exist)
- API type, versioning, endpoints
- Schemas, error codes, webhooks
→ Output: [OUTPUT_PATH]/analysis/api-contract-analysis.md

## 1.13 Dependency Analysis (if dependency review needed)
- Package dependencies, internal modules
- Circular dependencies, vulnerabilities
→ Output: [OUTPUT_PATH]/analysis/dependency-analysis.md

---

# STEP 2: Generate Diagrams

Generate DrawIO diagrams from existing analysis files only.

## 2.1 Architecture Diagram
→ Input: architecture-analysis.md
→ Output: [OUTPUT_PATH]/diagrams/architecture-overview.drawio

## 2.2 ERD Diagram (if data-models-analysis.md exists)
→ Output: [OUTPUT_PATH]/diagrams/entity-erd.drawio

## 2.3 Data Flow Diagram
→ Output: [OUTPUT_PATH]/diagrams/data-flow.drawio

## 2.4 Flowchart Diagram (if flowchart-analysis.md exists)
→ Output: [OUTPUT_PATH]/diagrams/flowchart-[process].drawio

## 2.5 Sequence Diagram (if sequence-analysis.md exists)
→ Output: [OUTPUT_PATH]/diagrams/sequence-[flow].drawio

## 2.6 Use Case Diagram (if use-case-analysis.md exists)
→ Output: [OUTPUT_PATH]/diagrams/use-case-[domain].drawio

## 2.7 State Machine Diagram (if state-machine-analysis.md exists)
→ Output: [OUTPUT_PATH]/diagrams/state-machine-[entity].drawio

## 2.8 Infrastructure Diagram (if infrastructure-analysis.md exists)
→ Output: [OUTPUT_PATH]/diagrams/infrastructure.drawio

## 2.9 C4 Model Diagrams
→ Output: [OUTPUT_PATH]/diagrams/c4-1-context.drawio
→ Output: [OUTPUT_PATH]/diagrams/c4-2-container.drawio
→ Output: [OUTPUT_PATH]/diagrams/c4-3-component-[name].drawio

## 2.10 DDD Context Map (if ddd-analysis.md exists)
→ Output: [OUTPUT_PATH]/diagrams/ddd-context-map.drawio

## 2.11 Security Diagram (if security-analysis.md exists)
→ Output: [OUTPUT_PATH]/diagrams/security-architecture.drawio

## 2.12 Dependency Diagram (if dependency-analysis.md exists)
→ Output: [OUTPUT_PATH]/diagrams/module-dependencies.drawio

---

## Diagram Requirements (apply to all)
- Use appropriate shapes for technology stack
- Apply protocol-based arrow colors
- Include arrow legend for all meaningful arrow styles
- Group related components in containers
- Follow all standards from diagram standards file

## Execution
1. STEP 1: Generate analysis .md files (skip non-applicable)
2. STEP 2: Generate .drawio diagrams (only for existing analysis)
3. Include mandatory arrow legends in all diagrams
```

---
---

# Documentation

## Overview

This prompt orchestrates a **two-step workflow**:
1. **Step 1**: Analyze project → Generate structured `.md` analysis files
2. **Step 2**: Read analysis files → Generate `.drawio` diagrams

### Important: Skip Irrelevant Analysis/Diagram Types

> **Note:** Not all analysis and diagram types apply to every project. **Skip any section that doesn't apply** to the target project.
>
> **Examples:**
> - No database/ORM → Skip Data Models / ERD
> - No cloud infrastructure / IaC → Skip Infrastructure Analysis
> - No Kubernetes → Skip K8s diagrams
> - No message queues or events → Skip async flow sections
> - Simple CRUD / monolith → Skip DDD Analysis
> - No external APIs → Skip API Contract Analysis
> - No complex workflows → Skip Flowchart / State Machine
>
> **Do not create empty or placeholder files.** Only generate analysis and diagrams for aspects that exist in the codebase.

---

## Detailed Workflow Reference

```
## Parameters (fill these values, they will be used throughout):
- PROJECT_PATH: __path_to_project_to_analyze__
- OUTPUT_PATH: __where_to_save_outputs__e_g___PROJECT_PATH_docs__
- CLAUDE_MD_PROJECT_ANALYSIS: __path_to_analysis_templates_file__
- CLAUDE_DIAGRAMS_STANDARD_FORMAT: __path_to_diagram_standards_file__

Use the parameter values above wherever you see [PARAMETER_NAME] below.

---

## Standards & Templates

- Analysis Templates: [CLAUDE_MD_PROJECT_ANALYSIS]
- Diagram Standards: [CLAUDE_DIAGRAMS_STANDARD_FORMAT]

## Project

Path: [PROJECT_PATH]

## Output Location

Save all outputs to: [OUTPUT_PATH]

---

# STEP 1: Project Analysis

Analyze the project and generate structured markdown analysis files.

## 1.1 Architecture Analysis
Analyze and document:
- Application overview (name, type, language, framework)
- Architecture pattern (MVC, Clean, Hexagonal, Layered)
- Entry points (API, UI, CLI, jobs)
- Components & modules with dependencies
- External integrations (APIs, SDKs, services)
- Cross-cutting concerns (auth, logging, caching)

**Output**: [OUTPUT_PATH]/analysis/architecture-analysis.md

## 1.2 Data Models / ERD Analysis
Analyze and document:
- Database type(s) and ORM
- All entities/tables with columns, types, constraints
- Primary keys and foreign keys
- Relationships (1:1, 1:N, N:M) with FK columns
- Indexes and enums
- Entity groupings (core, transactional, reference)

**Output**: [OUTPUT_PATH]/analysis/data-models-analysis.md

## 1.3 Data Flow Analysis
Analyze and document:
- User input flows (trigger → steps → output → storage)
- API request flows (endpoint → processing → response)
- Background job flows
- Event/message flows (producer → payload → consumers)
- Data transformation points

**Output**: [OUTPUT_PATH]/analysis/data-flow-analysis.md

## 1.4 Infrastructure Analysis
Analyze and document:
- Cloud provider(s) and regions
- Compute resources (VMs, containers, serverless)
- Networking (VPC, subnets, load balancers, firewall)
- Data storage (databases, object storage, cache)
- Security (IAM, secrets, certificates)
- Integration services (queues, event streaming, API gateway)
- Monitoring and CI/CD

**Output**: [OUTPUT_PATH]/analysis/infrastructure-analysis.md

## 1.5 C4 Model Analysis
Analyze and document:
- Level 1 Context: System, actors, external systems
- Level 2 Containers: Deployable units, technologies, relationships
- Level 3 Components: Per container, components and interfaces

**Output**: [OUTPUT_PATH]/analysis/c4-model-analysis.md

## 1.6 Flowchart / Process Analysis (if applicable)
Analyze and document:
- Business processes and workflows
- Decision points with branching logic
- User journeys and UI flows
- Approval workflows
- State transitions (simple)

**Output**: [OUTPUT_PATH]/analysis/flowchart-analysis.md

## 1.7 Sequence Diagram Analysis (if applicable)
Analyze and document:
- API interaction sequences
- Service-to-service communication flows
- Database interaction sequences
- External system integration flows
- Asynchronous message flows

**Output**: [OUTPUT_PATH]/analysis/sequence-analysis.md

## 1.8 Use Case Analysis (if applicable)
Analyze and document:
- Actors (persons, systems)
- Use cases with flows (main, alternative, exception)
- Include/extend relationships
- Actor-use case matrix

**Output**: [OUTPUT_PATH]/analysis/use-case-analysis.md

## 1.9 State Machine Analysis (if applicable)
Analyze and document:
- Stateful entities and their states
- State transitions with triggers and guards
- Composite and parallel states
- State history and persistence

**Output**: [OUTPUT_PATH]/analysis/state-machine-analysis.md

## 1.10 DDD Analysis (if applicable)
Analyze and document:
- Domain and subdomains (core, supporting, generic)
- Bounded contexts with ubiquitous language
- Context mapping (relationships and patterns)
- Aggregates, entities, value objects
- Domain events and services

**Output**: [OUTPUT_PATH]/analysis/ddd-analysis.md

## 1.11 Security Analysis (if applicable)
Analyze and document:
- Authentication and authorization methods
- Security boundaries and trust zones
- Data protection (encryption, masking)
- Threat model (STRIDE)
- Security controls and secrets management

**Output**: [OUTPUT_PATH]/analysis/security-analysis.md

## 1.12 API Contract Analysis (if applicable)
Analyze and document:
- API type and versioning strategy
- Endpoints with auth requirements
- Request/response schemas
- Error codes and pagination
- Webhooks if applicable

**Output**: [OUTPUT_PATH]/analysis/api-contract-analysis.md

## 1.13 Dependency Analysis (if applicable)
Analyze and document:
- Package dependencies (production/dev)
- Internal module dependencies
- Circular dependencies
- Outdated packages and vulnerabilities
- License compliance

**Output**: [OUTPUT_PATH]/analysis/dependency-analysis.md

---

# STEP 2: Generate Diagrams

Read the analysis files and generate DrawIO diagrams following all standards.

## 2.1 Architecture Diagram
- **Input**: [OUTPUT_PATH]/analysis/architecture-analysis.md
- **Requirements**:
  - Show all components, layers, integrations
  - Use appropriate shapes for technology stack
  - Apply protocol-based arrow colors
  - Include arrow legend
  - Group related components
- **Output**: [OUTPUT_PATH]/diagrams/architecture-overview.drawio

## 2.2 ERD Diagram
- **Input**: [OUTPUT_PATH]/analysis/data-models-analysis.md
- **Requirements**:
  - Use swimlane table format with PK/FK indicators
  - Show relationships with ERone/ERmany cardinality
  - Group tables by domain/database
  - Include PK/FK legend and arrow legend
  - Highlight central entities
- **Output**: [OUTPUT_PATH]/diagrams/entity-erd.drawio

## 2.3 Data Flow Diagram
- **Input**: [OUTPUT_PATH]/analysis/data-flow-analysis.md
- **Requirements**:
  - Show data movement from input to storage
  - Include processing steps and transformations
  - Use flow animation for active paths
  - Differentiate sync vs async (solid vs dashed)
  - Include arrow legend
- **Output**: [OUTPUT_PATH]/diagrams/data-flow.drawio

## 2.4 Infrastructure Diagram
- **Input**: [OUTPUT_PATH]/analysis/infrastructure-analysis.md
- **Requirements**:
  - Use official cloud provider shapes
  - Show network hierarchy (Region → VPC → Subnet)
  - Apply protocol-based arrow colors
  - Include arrow legend
  - Show security boundaries
- **Output**: [OUTPUT_PATH]/diagrams/infrastructure.drawio

## 2.5 C4 Model Diagrams
- **Input**: [OUTPUT_PATH]/analysis/c4-model-analysis.md
- **Requirements**:
  - Level 1 Context → [OUTPUT_PATH]/diagrams/c4-1-context.drawio
  - Level 2 Container → [OUTPUT_PATH]/diagrams/c4-2-container.drawio
  - Level 3 Component → [OUTPUT_PATH]/diagrams/c4-3-component-[name].drawio
  - Follow C4 color conventions
  - Include arrow legends

## 2.6 Flowchart Diagram (if flowchart analysis exists)
- **Input**: [OUTPUT_PATH]/analysis/flowchart-analysis.md
- **Requirements**:
  - Use standard flowchart shapes (rounded rect, diamond, parallelogram)
  - Show decision branches (Yes/No paths)
  - Use swimlanes for multi-actor processes
  - Include error/exception paths
  - Include arrow legend
- **Output**: [OUTPUT_PATH]/diagrams/flowchart-[process-name].drawio

## 2.7 Sequence Diagram (if sequence analysis exists)
- **Input**: [OUTPUT_PATH]/analysis/sequence-analysis.md
- **Requirements**:
  - Use UML sequence notation (lifelines, activation bars)
  - Solid arrows for sync, dashed for async
  - Show self-calls and return messages
  - Include opt/alt/loop fragments
  - Include arrow legend
- **Output**: [OUTPUT_PATH]/diagrams/sequence-[flow-name].drawio

## 2.8 Use Case Diagram (if use case analysis exists)
- **Input**: [OUTPUT_PATH]/analysis/use-case-analysis.md
- **Requirements**:
  - Stick figures for actors, ovals for use cases
  - Show include/extend relationships
  - Group by subsystem/package
  - Include arrow legend
- **Output**: [OUTPUT_PATH]/diagrams/use-case-[domain].drawio

## 2.9 State Machine Diagram (if state machine analysis exists)
- **Input**: [OUTPUT_PATH]/analysis/state-machine-analysis.md
- **Requirements**:
  - Rounded rectangles for states
  - Initial/final state markers
  - Label transitions with trigger [guard] / action
  - Show composite states if applicable
  - Include arrow legend
- **Output**: [OUTPUT_PATH]/diagrams/state-machine-[entity].drawio

## 2.10 DDD Context Map (if DDD analysis exists)
- **Input**: [OUTPUT_PATH]/analysis/ddd-analysis.md
- **Requirements**:
  - Show bounded contexts as containers
  - Label relationship patterns
  - Show aggregates and domain events
  - Use colors for Core/Supporting/Generic
  - Include arrow legend
- **Output**: [OUTPUT_PATH]/diagrams/ddd-context-map.drawio

## 2.11 Security Architecture Diagram (if security analysis exists)
- **Input**: [OUTPUT_PATH]/analysis/security-analysis.md
- **Requirements**:
  - Show trust zones with colored regions
  - Mark entry points and security controls
  - Show data flow with sensitivity levels
  - Distinct colors for public/DMZ/private zones
  - Include arrow legend
- **Output**: [OUTPUT_PATH]/diagrams/security-architecture.drawio

## 2.12 Module Dependency Diagram (if dependency analysis exists)
- **Input**: [OUTPUT_PATH]/analysis/dependency-analysis.md
- **Requirements**:
  - Show internal modules as rectangles
  - Arrows indicate dependency direction
  - Highlight circular dependencies in red
  - Group by layer/domain
  - Include arrow legend
- **Output**: [OUTPUT_PATH]/diagrams/module-dependencies.drawio

---

## Execution Instructions

1. Execute STEP 1 - generate analysis .md files **only for applicable sections**
2. Execute STEP 2 - generate .drawio diagrams **only for existing analysis files**
3. **Skip any section** where the project lacks relevant information
4. Ensure all diagrams include mandatory arrow legends
5. Follow all standards from the provided template files
```

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| `[PROJECT_PATH]` | Path to project being analyzed |
| `[OUTPUT_PATH]` | Where to save analysis and diagrams |
| `[CLAUDE_MD_PROJECT_ANALYSIS]` | Path to analysis templates file |
| `[CLAUDE_DIAGRAMS_STANDARD_FORMAT]` | Path to diagram standards file |

---

## Example Usage

```
## Standards & Templates

- Analysis Templates: D:\Standards\CLAUDE-MD-PROJECT-ANALYSIS.md
- Diagram Standards: D:\Standards\CLAUDE-DIAGRAMS-STANDARD-FORMAT.md

## Project

Path: D:\Projects\my-ecommerce-app

## Output Location

Save all outputs to: D:\Projects\my-ecommerce-app\docs

---

# STEP 1: Project Analysis
[... executes analysis, generates .md files ...]

# STEP 2: Generate Diagrams
[... reads .md files, generates .drawio diagrams ...]
```

### Expected Output Structure

> **Note:** Only applicable files will be generated. Skip sections that don't apply.

```
docs/
├── analysis/
│   ├── architecture-analysis.md
│   ├── data-models-analysis.md
│   ├── data-flow-analysis.md
│   ├── flowchart-analysis.md          # if workflows exist
│   ├── sequence-analysis.md           # if complex interactions exist
│   ├── use-case-analysis.md           # if documenting requirements
│   ├── state-machine-analysis.md      # if stateful entities exist
│   ├── infrastructure-analysis.md     # if cloud/IaC exists
│   ├── c4-model-analysis.md
│   ├── ddd-analysis.md                # if DDD patterns used
│   ├── security-analysis.md           # if security review needed
│   ├── api-contract-analysis.md       # if APIs exist
│   └── dependency-analysis.md         # if dependency review needed
└── diagrams/
    ├── architecture-overview.drawio
    ├── entity-erd.drawio
    ├── data-flow.drawio
    ├── flowchart-[process].drawio
    ├── sequence-[flow].drawio
    ├── use-case-[domain].drawio
    ├── state-machine-[entity].drawio
    ├── infrastructure.drawio
    ├── c4-1-context.drawio
    ├── c4-2-container.drawio
    ├── c4-3-component-[name].drawio
    ├── ddd-context-map.drawio
    ├── security-architecture.drawio
    └── module-dependencies.drawio
```
