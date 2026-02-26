# Project Analysis & Diagram Generation Prompt

## Overview

This prompt orchestrates a **two-step workflow**:
1. **Step 1**: Analyze project → Generate structured `.md` analysis files
2. **Step 2**: Read analysis files → Generate `.drawio` diagrams

---

## Full Workflow Prompt

```
## Parameters (fill these values, they will be used throughout):
- PROJECT_PATH: __path_to_project_to_analyze__
- OUTPUT_PATH: __where_to_save_outputs__e_g___PROJECT_PATH_docs__
- CLAUDE_MD_PROJECT_ANALYSIS: __ath_to_analysis_templates_file__
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

## 1.6 DDD Analysis (if applicable)
Analyze and document:
- Domain and subdomains (core, supporting, generic)
- Bounded contexts with ubiquitous language
- Context mapping (relationships and patterns)
- Aggregates, entities, value objects
- Domain events and services

**Output**: [OUTPUT_PATH]/analysis/ddd-analysis.md

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

## 2.6 DDD Context Map (if DDD analysis exists)
- **Input**: [OUTPUT_PATH]/analysis/ddd-analysis.md
- **Requirements**:
  - Show bounded contexts as containers
  - Label relationship patterns
  - Show aggregates and domain events
  - Use colors for Core/Supporting/Generic
  - Include arrow legend
- **Output**: [OUTPUT_PATH]/diagrams/ddd-context-map.drawio

---

## Execution Instructions

1. Execute STEP 1 completely - generate all analysis .md files
2. Execute STEP 2 completely - generate all .drawio diagrams from analysis
3. Ensure all diagrams include mandatory arrow legends
4. Follow all standards from the provided template files
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

```
docs/
├── analysis/
│   ├── architecture-analysis.md
│   ├── data-models-analysis.md
│   ├── data-flow-analysis.md
│   ├── infrastructure-analysis.md
│   ├── c4-model-analysis.md
│   └── ddd-analysis.md
└── diagrams/
    ├── architecture-overview.drawio
    ├── entity-erd.drawio
    ├── data-flow.drawio
    ├── infrastructure.drawio
    ├── c4-1-context.drawio
    ├── c4-2-container.drawio
    ├── c4-3-component-[name].drawio
    └── ddd-context-map.drawio
```
