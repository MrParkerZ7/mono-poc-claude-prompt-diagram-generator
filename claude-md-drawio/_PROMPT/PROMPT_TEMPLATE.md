# Project Analysis & Diagram Generation Prompt

> **Instructions:** Copy the entire block below, fill in the 4 parameters at the top, then paste to Claude.

```
## Project Analysis & Diagram Generation

### Parameters (FILL THESE FIRST):
- PROJECT_PATH: D:\Programing\GEC-SCF\
- OUTPUT_PATH: D:\Programing\GEC-SCF\
- PROMPT_PROJECT_ANALYSIS: D:\Programing\mono-sample-claude-project-specific-instruction\claude-md-drawio\_PROMPT\PROMPT-PROJECT-ANALYSIS.md
- PROMPT_DIAGRAM_FORMAT: D:\Programing\mono-sample-claude-project-specific-instruction\claude-md-drawio\_PROMPT\PROMPT-DIAGRAM-FORMAT.md

### Standards & Templates
- Analysis Templates: [PROMPT_PROJECT_ANALYSIS]
- Diagram Standards: [PROMPT_DIAGRAM_FORMAT]

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

