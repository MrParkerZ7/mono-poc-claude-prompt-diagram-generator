# Project Analysis Templates for Diagram Generation

## Overview

This document provides **two-step workflow templates** for generating accurate DrawIO diagrams:

1. **Step 1: Analysis** - Analyze project and generate structured `.md` documentation
2. **Step 2: Diagram Generation** - Use the `.md` as input to generate `.drawio` files

### Why Two-Step Workflow?

| Benefit | Description |
|---------|-------------|
| **Verification** | Review and correct analysis before diagram generation |
| **Accuracy** | Structured format forces explicit decisions, reduces hallucination |
| **Reusability** | Update `.md` file, regenerate diagrams without re-analysis |
| **Documentation** | The `.md` files serve as valuable documentation artifacts |
| **Multi-diagram** | One analysis can generate multiple diagram types |

### Important: Skip Irrelevant Analysis Types

> **Note:** Not all analysis types apply to every project. If the target project does not have sufficient information for a specific analysis type, **skip creating that `.md` file entirely**.
>
> **Examples:**
> - No database/ORM → Skip Data Models / ERD Analysis
> - No cloud infrastructure / IaC files → Skip Infrastructure Analysis
> - No message queues or events → Skip event-related sections in Data Flow
> - Simple CRUD app → Skip DDD Analysis
> - No external APIs → Skip API Contract Analysis
> - Single-module project → Skip Dependency Analysis (internal modules)
>
> **Do not create empty or placeholder analysis files.** Only generate analysis for aspects that exist in the codebase.
>
> **IMPORTANT: README.md is ALWAYS required.** The `README.md` file serves as the main project summary and index for all analysis documents and diagrams. It must be created for every project regardless of which other analysis types apply.

---

## Table of Contents

### Analysis Prompts (Step 1)
- [Architecture Analysis](#1-architecture-analysis-prompt)
- [Data Models / ERD Analysis](#2-data-models--erd-analysis-prompt)
- [Data Flow Analysis](#3-data-flow-analysis-prompt)
- [Flowchart / Process Analysis](#4-flowchart--process-analysis-prompt)
- [Sequence Diagram Analysis](#5-sequence-diagram-analysis-prompt)
- [Use Case Analysis](#6-use-case-analysis-prompt)
- [State Machine Analysis](#7-state-machine-analysis-prompt)
- [Infrastructure Analysis](#8-infrastructure-analysis-prompt)
- [C4 Model Analysis](#9-c4-model-analysis-prompt)
- [Domain-Driven Design Analysis](#10-domain-driven-design-analysis-prompt)
- [Security Analysis](#11-security-analysis-prompt)
- [API Contract Analysis](#12-api-contract-analysis-prompt)
- [Dependency Analysis](#13-dependency-analysis-prompt)

### Diagram Generation Prompts (Step 2)
- [Generate Diagrams from Analysis](#step-2-generate-diagrams-from-analysis)

### Output Templates
- [**README (Project Summary) - REQUIRED**](#readme-project-summary-output-template)
- [Architecture Analysis Output Template](#architecture-analysis-output-template)
- [Data Models Output Template](#data-models-output-template)
- [Data Flow Output Template](#data-flow-output-template)
- [Flowchart Output Template](#flowchart-output-template)
- [Sequence Diagram Output Template](#sequence-diagram-output-template)
- [Use Case Output Template](#use-case-output-template)
- [State Machine Output Template](#state-machine-output-template)
- [Infrastructure Output Template](#infrastructure-output-template)
- [C4 Model Output Template](#c4-model-output-template)
- [DDD Output Template](#ddd-output-template)
- [Security Analysis Output Template](#security-analysis-output-template)
- [API Contract Output Template](#api-contract-output-template)
- [Dependency Analysis Output Template](#dependency-analysis-output-template)

---

# Step 1: Analysis Prompts

---

## 1. Architecture Analysis Prompt

Use this to analyze application architecture, layers, components, and integrations.

```
## Task: Architecture Analysis

Analyze the project and generate a structured architecture analysis document.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Examine the entire codebase and document:

#### 1. Application Overview
- Project name and description
- Primary programming language(s)
- Framework(s) and major libraries
- Application type (Web API, CLI, Desktop, Mobile, etc.)

#### 2. Architecture Pattern
- Identified pattern (MVC, MVVM, Clean Architecture, Hexagonal, Layered, etc.)
- Layer definitions and responsibilities
- Dependency direction

#### 3. Entry Points
- API endpoints (REST, GraphQL, gRPC)
- UI entry points (pages, screens)
- CLI commands
- Background jobs/workers
- Event handlers

#### 4. Components & Modules
For each major component/module:
- Name and purpose
- Dependencies (what it uses)
- Dependents (what uses it)
- Key classes/functions

#### 5. External Integrations
- Third-party APIs (with endpoints if visible)
- SDKs and external services
- Authentication providers
- Payment gateways
- Email/SMS services
- Cloud services

#### 6. Cross-Cutting Concerns
- Authentication & Authorization mechanism
- Logging strategy
- Error handling approach
- Caching strategy
- Configuration management

### Output Format
Follow the [Architecture Analysis Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/architecture-analysis.md
```

---

## 2. Data Models / ERD Analysis Prompt

Use this to analyze entities, relationships, and database schema.

```
## Task: Data Models & ERD Analysis

Analyze the project and generate a structured data models document for ERD generation.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Examine all data models, entities, and database schemas:

#### 1. Database Overview
- Database type(s) (PostgreSQL, MySQL, MongoDB, Redis, etc.)
- ORM/ODM used (Entity Framework, Prisma, Mongoose, etc.)
- Connection configuration location

#### 2. Entities / Tables
For EACH entity/table, document:
- **Name**: Table/collection name
- **Description**: Purpose of this entity
- **Properties/Columns**:
  | Column | Type | Constraints | Description |
  |--------|------|-------------|-------------|
  | id | UUID | PK, NOT NULL | Primary identifier |
  | ... | ... | ... | ... |

#### 3. Relationships
For EACH relationship:
- **From Entity**: Source table
- **To Entity**: Target table
- **Type**: 1:1, 1:N, N:M
- **FK Column**: Foreign key column name
- **Cascade**: Delete/Update behavior
- **Description**: Business meaning

#### 4. Indexes
- Index name
- Table
- Columns
- Type (unique, composite, full-text)

#### 5. Enums / Lookup Tables
- Enum name
- Possible values
- Used in which entities

#### 6. Database Groupings
Group related tables:
- Core entities (e.g., users, accounts)
- Transactional entities (e.g., orders, payments)
- Reference data (e.g., countries, categories)
- Audit/logging tables

### Output Format
Follow the [Data Models Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/data-models-analysis.md
```

---

## 3. Data Flow Analysis Prompt

Use this to analyze how data moves through the system.

```
## Task: Data Flow Analysis

Analyze the project and document all data flows from input to output.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Trace data movement through the entire system:

#### 1. User Input Flows
For each user-facing feature:
- **Flow Name**: Descriptive name (e.g., "User Registration Flow")
- **Trigger**: What initiates the flow (button click, API call, scheduled job)
- **Input**: Data received (with types)
- **Steps**: Sequential processing steps
  1. Step name → Component → Action → Output
  2. ...
- **Output**: Final result
- **Storage**: Where data is persisted

#### 2. API Request Flows
For each API endpoint:
- **Endpoint**: Method + Path
- **Request**: Input payload structure
- **Processing Steps**:
  1. Validation → Component
  2. Business Logic → Component
  3. Data Access → Component
- **Response**: Output payload structure
- **Side Effects**: Events emitted, notifications sent

#### 3. Background Processing Flows
For each background job/worker:
- **Job Name**: Identifier
- **Trigger**: Schedule, event, queue message
- **Input Source**: Where data comes from
- **Processing**: What happens
- **Output**: Results, side effects

#### 4. Event/Message Flows
For each event type:
- **Event Name**: Identifier
- **Producer**: What emits this event
- **Payload**: Data structure
- **Consumers**: What processes this event
- **Actions**: What each consumer does

#### 5. Data Transformation Points
- Where data shape changes
- Mapping/conversion logic locations
- Serialization/deserialization points

### Output Format
Follow the [Data Flow Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/data-flow-analysis.md
```

---

## 4. Flowchart / Process Analysis Prompt

Use this to analyze business processes, workflows, and decision logic.

```
## Task: Flowchart / Process Analysis

Analyze the project and document all business processes and workflows.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Identify and document all process flows in the system:

#### 1. Business Processes
For each business process:
- **Process Name**: Descriptive name (e.g., "Order Checkout Process")
- **Description**: What this process accomplishes
- **Trigger**: What initiates the process
- **Actors**: Who/what participates
- **Steps**:
  1. Step name
  2. Step name
  3. ...
- **End States**: Possible outcomes

#### 2. Decision Points
For each decision in a process:
- **Decision**: Question being asked (e.g., "Is payment valid?")
- **Condition**: Logic evaluated
- **Yes Path**: What happens if true
- **No Path**: What happens if false
- **Location**: Which process contains this decision

#### 3. Workflow Types
Categorize workflows:
- **User Workflows**: User-initiated actions (registration, checkout, profile update)
- **System Workflows**: Automated processes (scheduled jobs, triggers)
- **Approval Workflows**: Multi-step approvals (refund request, content moderation)
- **Error Handling Workflows**: Exception handling paths

#### 4. Process Details
For each major process, document:
- **Swimlanes**: Actors/systems involved
- **Parallel Paths**: Concurrent activities
- **Loops**: Iterative steps
- **Subprocesses**: Nested workflows
- **Timeouts**: Time-based transitions
- **Error Paths**: Exception handling

#### 5. User Journeys
For key user flows:
- **Journey Name**: (e.g., "First-time Buyer Journey")
- **Persona**: Target user type
- **Goal**: What user wants to achieve
- **Steps**: Sequential actions with decision points
- **Success Criteria**: How success is measured
- **Alternative Paths**: Variations and edge cases

#### 6. State Transitions (Simple)
For entities with clear states:
- **Entity**: What has states (Order, User, Document)
- **States**: Possible states
- **Transitions**: State changes with triggers

### Output Format
Follow the [Flowchart Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/flowchart-analysis.md
```

---

## 5. Sequence Diagram Analysis Prompt

Use this to analyze method call sequences, API interactions, and component communications.

```
## Task: Sequence Diagram Analysis

Analyze the project and document interaction sequences between components.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Document all significant interaction sequences:

#### 1. API Interaction Sequences
For each major API operation:
- **Sequence Name**: Descriptive name (e.g., "User Login Sequence")
- **Trigger**: What initiates the sequence
- **Participants**: All components/services involved
- **Messages**: Ordered list of calls
  | # | From | To | Message | Return |
  |---|------|-----|---------|--------|
  | 1 | Client | API Gateway | POST /login | - |
  | 2 | API Gateway | AuthService | validateCredentials() | token |
  | ... | ... | ... | ... | ... |

#### 2. Service-to-Service Sequences
For each internal service interaction:
- **Sequence Name**: Identifier
- **Participants**: Services involved
- **Synchronous Calls**: Request-response messages
- **Asynchronous Calls**: Fire-and-forget, events
- **Return Values**: What each call returns

#### 3. Database Interaction Sequences
For complex data operations:
- **Operation**: What is being done
- **Participants**: Service, Repository, Database
- **Queries**: SQL/NoSQL operations in order
- **Transactions**: Transaction boundaries

#### 4. External System Sequences
For third-party integrations:
- **Integration**: External system name
- **Sequence**: Full call flow
- **Request/Response**: Payloads exchanged
- **Error Handling**: Failure scenarios

#### 5. Asynchronous Sequences
For message-based flows:
- **Flow Name**: Identifier
- **Producer**: Message sender
- **Queue/Topic**: Message channel
- **Consumer(s)**: Message handlers
- **Acknowledgment**: ACK/NACK behavior

#### 6. Activation/Lifeline Notes
- **Long-running Operations**: Mark with activation bars
- **Parallel Processing**: Note concurrent executions
- **Optional Flows**: Conditional sequences
- **Loop Sequences**: Repeated interactions

### Output Format
Follow the [Sequence Diagram Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/sequence-analysis.md
```

---

## 6. Use Case Analysis Prompt

Use this to analyze user stories, actor interactions, and system requirements.

```
## Task: Use Case Analysis

Analyze the project and document all use cases and actor interactions.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Identify and document all use cases:

#### 1. Actors
For each actor:
- **Actor Name**: Identifier (e.g., "Customer", "Admin", "System")
- **Type**: Person, External System, Time-based
- **Description**: Role description
- **Goals**: What the actor wants to achieve
- **Permissions**: Access level / capabilities

#### 2. Use Cases
For each use case:
- **Use Case ID**: UC-001
- **Name**: Action phrase (e.g., "Place Order")
- **Actor(s)**: Primary and secondary actors
- **Description**: Brief summary
- **Preconditions**: Required state before execution
- **Postconditions**: State after successful execution
- **Main Flow**:
  1. Actor action
  2. System response
  3. ...
- **Alternative Flows**: Variations
- **Exception Flows**: Error scenarios
- **Business Rules**: Applicable rules
- **Frequency**: How often this occurs

#### 3. Use Case Relationships
Document relationships:
- **Includes**: UC-001 includes UC-002
- **Extends**: UC-003 extends UC-001
- **Generalizations**: Actor hierarchies

#### 4. Use Case Groups
Categorize by domain area:
- **Authentication**: Login, Register, Password Reset
- **Order Management**: Place Order, Cancel Order, Track Order
- **Admin Functions**: Manage Users, View Reports

#### 5. Actor-Use Case Matrix
| Use Case | Customer | Admin | System |
|----------|----------|-------|--------|
| Place Order | Primary | - | - |
| Manage Users | - | Primary | - |
| Send Notifications | - | - | Primary |

#### 6. Non-Functional Requirements
Per use case where applicable:
- **Performance**: Response time requirements
- **Security**: Authentication/authorization needs
- **Availability**: Uptime requirements

### Output Format
Follow the [Use Case Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/use-case-analysis.md
```

---

## 7. State Machine Analysis Prompt

Use this to analyze entity states, transitions, and lifecycle management.

```
## Task: State Machine Analysis

Analyze the project and document all state machines and entity lifecycles.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Document all stateful entities and their transitions:

#### 1. Stateful Entities
Identify entities with states:
- **Entity Name**: (e.g., Order, User, Document)
- **State Field**: Code location of state
- **State Type**: Enum, String, Integer
- **Initial State**: Starting state

#### 2. States
For each entity, document all states:
| State | Description | Entry Action | Exit Action |
|-------|-------------|--------------|-------------|
| Draft | Initial creation | Initialize fields | Validate |
| Pending | Awaiting action | Notify user | - |
| Active | In use | Start processing | Stop processing |
| Completed | Finished | Archive | - |

#### 3. Transitions
For each transition:
- **From State**: Source state
- **To State**: Target state
- **Trigger**: What causes transition (event, action, time)
- **Guard Condition**: Boolean condition required
- **Action**: What happens during transition
- **Code Location**: Where transition is implemented

#### 4. State Machine Diagram Data
```
[Initial] ──create──▶ [Draft]
                         │
                         │ submit
                         ▼
                      [Pending]
                         │
            ┌────────────┼────────────┐
            │ approve    │ reject     │ timeout
            ▼            ▼            ▼
        [Active]    [Rejected]   [Expired]
            │
            │ complete
            ▼
       [Completed]
```

#### 5. Composite States
For nested states:
- **Parent State**: Container state
- **Child States**: Nested states
- **Entry/Exit**: How nesting works

#### 6. Parallel States
For concurrent state regions:
- **Entity**: What has parallel states
- **Regions**: Independent state areas
- **Synchronization**: How regions interact

#### 7. State History
- **Entry Point**: Where state is loaded
- **Persistence**: How state is stored
- **Audit Trail**: State change logging

### Output Format
Follow the [State Machine Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/state-machine-analysis.md
```

---

## 8. Infrastructure Analysis Prompt

Use this to analyze cloud resources, networking, and deployment.

```
## Task: Infrastructure Analysis

Analyze the project's infrastructure configuration and deployment setup.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Examine infrastructure-as-code, config files, and deployment scripts:

#### 1. Cloud Provider(s)
- Primary cloud (AWS, Azure, GCP, On-Premises)
- Secondary/hybrid setup if any
- Account/subscription structure

#### 2. Compute Resources
For each compute resource:
- **Name**: Resource identifier
- **Type**: VM, Container, Serverless, K8s Pod
- **Specs**: Size, CPU, Memory
- **Scaling**: Auto-scaling configuration
- **Network**: Subnet, security groups

#### 3. Networking
- **VPC/VNet**: CIDR blocks, regions
- **Subnets**: Public/private, availability zones
- **Load Balancers**: Type, listeners, targets
- **DNS**: Domains, records
- **Firewall/Security Groups**: Inbound/outbound rules
- **VPN/Peering**: Connections to other networks

#### 4. Data Storage
- **Databases**: Type, instance size, replicas
- **Object Storage**: Buckets, lifecycle policies
- **Cache**: Redis/Memcached instances
- **File Storage**: NFS, EFS, Azure Files

#### 5. Security
- **Identity**: IAM roles, service accounts
- **Secrets Management**: Key Vault, Secrets Manager
- **Certificates**: SSL/TLS configuration
- **Encryption**: At-rest, in-transit

#### 6. Integration Services
- **Message Queues**: SQS, Service Bus, RabbitMQ
- **Event Streaming**: Kafka, Event Hub, Kinesis
- **API Gateway**: Configuration, routes

#### 7. Monitoring & Logging
- **Monitoring**: CloudWatch, Azure Monitor, Prometheus
- **Logging**: Log aggregation, retention
- **Alerting**: Alert rules, notification channels

#### 8. CI/CD Pipeline
- **Source**: Repository, branches
- **Build**: Build steps, artifacts
- **Deploy**: Deployment targets, strategies

### Output Format
Follow the [Infrastructure Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/infrastructure-analysis.md
```

---

## 9. C4 Model Analysis Prompt

Use this for C4 Model (Context, Container, Component) analysis.

```
## Task: C4 Model Analysis

Analyze the project using the C4 Model framework for software architecture.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

#### Level 1: System Context
Document the big picture:
- **System Name**: The software system being analyzed
- **System Description**: What it does, key capabilities
- **Users/Actors**:
  | Actor | Type | Description | Interactions |
  |-------|------|-------------|--------------|
  | Customer | Person | End user | Browse, purchase, review |
  | Admin | Person | System administrator | Manage users, configure |
  | ... | ... | ... | ... |
- **External Systems**:
  | System | Type | Description | Data Exchange |
  |--------|------|-------------|---------------|
  | Payment Gateway | External | Process payments | Send payment, receive confirmation |
  | Email Service | External | Send notifications | Send email requests |
  | ... | ... | ... | ... |

#### Level 2: Container Diagram
Document all containers (deployable units):
- **Containers**:
  | Container | Technology | Description | Responsibilities |
  |-----------|------------|-------------|------------------|
  | Web App | React/Next.js | Frontend SPA | User interface, client-side logic |
  | API Server | Node.js/Express | REST API | Business logic, data validation |
  | Database | PostgreSQL | Data storage | Persist user and business data |
  | Cache | Redis | Session/cache | Session storage, query caching |
  | ... | ... | ... | ... |

- **Container Relationships**:
  | From | To | Protocol | Description |
  |------|-----|----------|-------------|
  | Web App | API Server | HTTPS/REST | API calls |
  | API Server | Database | TCP/SQL | Read/write data |
  | ... | ... | ... | ... |

#### Level 3: Component Diagram
For each major container, document components:
- **Container**: [Container Name]
- **Components**:
  | Component | Technology | Description | Interfaces |
  |-----------|------------|-------------|------------|
  | AuthController | Express Router | Handle auth endpoints | /login, /register, /logout |
  | UserService | TypeScript class | User business logic | createUser(), getUser() |
  | UserRepository | TypeORM Repository | Data access | save(), find(), delete() |
  | ... | ... | ... | ... |

- **Component Relationships**:
  | From | To | Description |
  |------|-----|-------------|
  | AuthController | UserService | Delegates business logic |
  | UserService | UserRepository | Data persistence |
  | ... | ... | ... |

### Output Format
Follow the [C4 Model Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/c4-model-analysis.md
```

---

## 10. Domain-Driven Design Analysis Prompt

Use this for DDD analysis including bounded contexts, aggregates, and domain events.

```
## Task: Domain-Driven Design Analysis

Analyze the project using Domain-Driven Design (DDD) patterns.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

#### 1. Domain Overview
- **Domain**: The business domain (e.g., E-commerce, Banking, Healthcare)
- **Subdomains**:
  | Subdomain | Type | Description |
  |-----------|------|-------------|
  | Orders | Core | Order processing and fulfillment |
  | Inventory | Supporting | Stock management |
  | Shipping | Generic | Delivery logistics |

#### 2. Bounded Contexts
For each bounded context:
- **Context Name**: Identifier
- **Description**: What this context handles
- **Ubiquitous Language**: Key terms and definitions
- **Owner Team**: Responsible team (if applicable)

#### 3. Context Mapping
Relationships between bounded contexts:
| Upstream | Downstream | Relationship Type | Description |
|----------|------------|-------------------|-------------|
| Orders | Shipping | Customer-Supplier | Orders triggers shipping |
| Inventory | Orders | Conformist | Orders follows Inventory model |

#### 4. Aggregates
For each aggregate:
- **Aggregate Name**: Root entity name
- **Aggregate Root**: The root entity
- **Entities**: Child entities within aggregate
- **Value Objects**: Immutable objects
- **Invariants**: Business rules that must always be true
- **Boundaries**: What's inside vs outside the aggregate

#### 5. Domain Events
| Event Name | Triggered By | Payload | Consumers |
|------------|--------------|---------|-----------|
| OrderPlaced | Order.place() | orderId, items, total | Inventory, Shipping, Notification |
| PaymentReceived | Payment.confirm() | paymentId, orderId, amount | Order, Accounting |

#### 6. Domain Services
| Service | Bounded Context | Responsibility | Methods |
|---------|-----------------|----------------|---------|
| PricingService | Orders | Calculate prices | calculateTotal(), applyDiscount() |
| ShippingCalculator | Shipping | Estimate delivery | estimateCost(), estimateDate() |

#### 7. Repositories
| Repository | Aggregate | Methods | Storage |
|------------|-----------|---------|---------|
| OrderRepository | Order | save(), findById(), findByCustomer() | PostgreSQL |
| ProductRepository | Product | save(), findById(), search() | PostgreSQL + Elasticsearch |

#### 8. Application Services
| Service | Use Cases | Coordinates |
|---------|-----------|-------------|
| OrderApplicationService | PlaceOrder, CancelOrder, GetOrderStatus | Order aggregate, PaymentService, InventoryService |

### Output Format
Follow the [DDD Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/ddd-analysis.md
```

---

## 11. Security Analysis Prompt

Use this to analyze security architecture, threat models, and access controls.

```
## Task: Security Analysis

Analyze the project's security architecture and identify security boundaries.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Document all security-related aspects:

#### 1. Authentication
- **Method(s)**: JWT, Session, OAuth2, API Keys
- **Provider**: Custom, Auth0, Cognito, etc.
- **Token Storage**: Where tokens are stored
- **Token Lifetime**: Expiration policies
- **Refresh Mechanism**: How tokens are refreshed
- **MFA**: Multi-factor authentication support

#### 2. Authorization
- **Model**: RBAC, ABAC, ACL, Custom
- **Roles**: Defined roles and permissions
  | Role | Permissions | Description |
  |------|-------------|-------------|
  | Admin | * | Full access |
  | User | read, write own | Standard user |
  | Guest | read public | Unauthenticated |
- **Permission Checks**: Where authorization is enforced
- **Resource Ownership**: How ownership is determined

#### 3. Security Boundaries
- **Trust Zones**: Network/logical boundaries
- **Entry Points**: All system entry points
- **Sensitive Data Flows**: Where sensitive data travels
- **Encryption Boundaries**: In-transit, at-rest

#### 4. Data Protection
- **Sensitive Data Types**: PII, PHI, PCI, etc.
- **Encryption**: Algorithms, key management
- **Data Masking**: Where and how applied
- **Data Retention**: Policies and implementation
- **Data Sanitization**: Input validation locations

#### 5. Threat Model
For each identified threat:
| Threat | Category | Asset | Mitigation | Status |
|--------|----------|-------|------------|--------|
| SQL Injection | Injection | Database | Parameterized queries | Implemented |
| XSS | Injection | User sessions | Output encoding | Implemented |
| CSRF | Session | User actions | CSRF tokens | Implemented |

#### 6. Security Controls
- **Input Validation**: Where and how
- **Output Encoding**: Implementation
- **CORS Policy**: Configuration
- **CSP Headers**: Content Security Policy
- **Rate Limiting**: Limits and scope
- **Audit Logging**: What is logged

#### 7. Secrets Management
- **Secret Types**: API keys, passwords, certificates
- **Storage**: Vault, Secrets Manager, env vars
- **Rotation**: Rotation policies
- **Access**: Who/what can access secrets

#### 8. Compliance
- **Frameworks**: GDPR, HIPAA, PCI-DSS, SOC2
- **Requirements**: Applicable requirements
- **Implementation**: How requirements are met

### Output Format
Follow the [Security Analysis Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/security-analysis.md
```

---

## 12. API Contract Analysis Prompt

Use this to analyze API specifications, versioning, and endpoint documentation.

```
## Task: API Contract Analysis

Analyze the project's API contracts and specifications.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Document all API contracts:

#### 1. API Overview
- **API Type**: REST, GraphQL, gRPC, WebSocket
- **Base URL(s)**: API base paths
- **Version Strategy**: URL, header, query param
- **Current Version**: Active version(s)
- **Deprecated Versions**: Versions being phased out

#### 2. Authentication & Authorization
- **Auth Methods**: Bearer token, API key, OAuth
- **Auth Header**: Header format
- **Scopes/Permissions**: Required per endpoint

#### 3. Endpoints
For each endpoint:
| Method | Path | Summary | Auth | Request | Response |
|--------|------|---------|------|---------|----------|
| GET | /api/v1/users | List users | Bearer | Query params | User[] |
| POST | /api/v1/users | Create user | Bearer + admin | UserDTO | User |
| GET | /api/v1/users/{id} | Get user | Bearer | - | User |

#### 4. Request/Response Schemas
For each DTO/Schema:
```json
{
  "name": "UserDTO",
  "type": "object",
  "properties": {
    "email": { "type": "string", "format": "email", "required": true },
    "name": { "type": "string", "required": true },
    "role": { "type": "string", "enum": ["user", "admin"] }
  }
}
```

#### 5. Error Responses
Standard error format:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": []
  }
}
```

Error codes:
| Code | HTTP Status | Description |
|------|-------------|-------------|
| VALIDATION_ERROR | 400 | Input validation failed |
| UNAUTHORIZED | 401 | Authentication required |
| FORBIDDEN | 403 | Insufficient permissions |
| NOT_FOUND | 404 | Resource not found |

#### 6. Pagination
- **Strategy**: Offset, cursor, page-based
- **Parameters**: page, limit, cursor, etc.
- **Response Format**: Links, metadata

#### 7. Rate Limiting
- **Limits**: Requests per window
- **Headers**: Rate limit headers returned
- **Behavior**: What happens when exceeded

#### 8. Webhooks (if applicable)
| Event | Payload | Retry Policy |
|-------|---------|--------------|
| order.created | Order object | 3 retries, exponential backoff |
| payment.received | Payment object | 3 retries, exponential backoff |

#### 9. SDK/Client Generation
- **OpenAPI Spec**: Location of spec file
- **Generated Clients**: Available SDKs
- **Code Generation**: Tools used

### Output Format
Follow the [API Contract Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/api-contract-analysis.md
```

---

## 13. Dependency Analysis Prompt

Use this to analyze package dependencies, module coupling, and dependency health.

```
## Task: Dependency Analysis

Analyze the project's dependencies and module relationships.

### Project Path
[YOUR_PROJECT_PATH]

### Analysis Requirements

Document all dependencies and their relationships:

#### 1. Package Dependencies
For each dependency:
| Package | Version | Type | Purpose | License |
|---------|---------|------|---------|---------|
| express | ^4.18.0 | Production | Web framework | MIT |
| typescript | ^5.0.0 | Dev | Type checking | Apache-2.0 |
| jest | ^29.0.0 | Dev | Testing | MIT |

#### 2. Dependency Categories
Group by purpose:
- **Framework**: Core framework dependencies
- **Database**: ORM, drivers, migrations
- **Authentication**: Auth libraries
- **Validation**: Schema validation
- **Testing**: Test frameworks, mocks
- **Build Tools**: Bundlers, compilers
- **Utilities**: Helper libraries

#### 3. Dependency Tree
Key dependency chains:
```
app
├── express@4.18.0
│   ├── body-parser@1.20.0
│   └── cookie-parser@1.4.6
├── prisma@5.0.0
│   └── @prisma/client@5.0.0
└── ...
```

#### 4. Module Dependencies (Internal)
For each internal module:
| Module | Depends On | Depended By | Coupling |
|--------|------------|-------------|----------|
| UserService | UserRepository, AuthService | OrderService | Medium |
| OrderService | UserService, ProductService | - | High |

#### 5. Circular Dependencies
Identify circular references:
| Cycle | Modules Involved | Severity |
|-------|------------------|----------|
| 1 | A → B → C → A | High |
| 2 | X → Y → X | Medium |

#### 6. Outdated Dependencies
| Package | Current | Latest | Breaking Changes |
|---------|---------|--------|------------------|
| lodash | 4.17.0 | 4.17.21 | No |
| webpack | 4.46.0 | 5.88.0 | Yes |

#### 7. Security Vulnerabilities
| Package | Vulnerability | Severity | Fix Version |
|---------|---------------|----------|-------------|
| example-pkg | CVE-2023-XXXX | High | 1.2.3 |

#### 8. Dependency Metrics
- **Total Dependencies**: Count
- **Direct Dependencies**: Count
- **Transitive Dependencies**: Count
- **Dev Dependencies**: Count
- **Average Dependency Depth**: Number
- **Duplicate Packages**: List

#### 9. License Compliance
| License | Packages | Compatible |
|---------|----------|------------|
| MIT | 45 | Yes |
| Apache-2.0 | 12 | Yes |
| GPL-3.0 | 2 | Check |

### Output Format
Follow the [Dependency Analysis Output Template] structure.

### Output Path
Save to: [YOUR_PROJECT_PATH]/docs/analysis/dependency-analysis.md
```

---

# Step 2: Generate Diagrams from Analysis

After creating the analysis `.md` files, use these prompts to generate diagrams.

---

## Generate Architecture Diagram

```
## Task: Generate Architecture Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/architecture-analysis.md

### Requirements
1. Create a comprehensive architecture diagram based on the analysis
2. Include all components, layers, and integrations documented
3. Use appropriate shapes for the technology stack identified
4. Apply proper arrow colors for different connection types
5. Include arrow legend for all meaningful arrow styles
6. Group related components in containers

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/architecture-overview.drawio
```

---

## Generate ERD Diagram

```
## Task: Generate ERD Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/data-models-analysis.md

### Requirements
1. Create an ERD diagram based on the data models analysis
2. Use swimlane table format with PK/FK indicators
3. Show all relationships with proper cardinality (ERone, ERmany)
4. Group tables by database or domain area
5. Include legend for PK/FK notation
6. Include arrow legend for relationship types
7. Highlight central/core entities with distinct color

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/entity-erd.drawio
```

---

## Generate Data Flow Diagram

```
## Task: Generate Data Flow Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/data-flow-analysis.md

### Requirements
1. Create a data flow diagram based on the analysis
2. Show data movement from input sources to final storage
3. Include all processing steps and transformations
4. Use flow animation for active data paths
5. Differentiate sync vs async flows (solid vs dashed)
6. Include arrow legend for flow types
7. Show data stores, processes, and external entities

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/data-flow.drawio
```

---

## Generate Flowchart Diagram

```
## Task: Generate Flowchart Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/flowchart-analysis.md

### Requirements
1. Create flowchart diagrams based on the analysis
2. Use standard flowchart shapes:
   - Rounded rectangle: Start/End
   - Rectangle: Process/Action
   - Diamond: Decision
   - Parallelogram: Input/Output
3. Show decision branches clearly (Yes/No paths)
4. Use swimlanes for multi-actor processes
5. Include loop indicators where applicable
6. Show error/exception paths with distinct color
7. Include arrow legend for flow types

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/flowchart-[process-name].drawio
```

---

## Generate Sequence Diagram

```
## Task: Generate Sequence Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/sequence-analysis.md

### Requirements
1. Create sequence diagrams based on the analysis
2. Use standard UML sequence diagram notation:
   - Rectangles: Participants/Objects
   - Vertical dashed lines: Lifelines
   - Solid arrows: Synchronous calls
   - Dashed arrows: Return messages
   - Half arrowheads: Asynchronous calls
3. Show activation bars for processing time
4. Include opt/alt/loop fragments where applicable
5. Use self-calls for internal processing
6. Include arrow legend for message types

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/sequence-[flow-name].drawio
```

---

## Generate Use Case Diagram

```
## Task: Generate Use Case Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/use-case-analysis.md

### Requirements
1. Create use case diagram based on the analysis
2. Use standard UML use case notation:
   - Stick figures: Actors
   - Ovals: Use cases
   - System boundary: Rectangle
3. Show relationships:
   - Association: Solid line (actor to use case)
   - Include: Dashed arrow with <<include>>
   - Extend: Dashed arrow with <<extend>>
   - Generalization: Solid arrow with triangle
4. Group use cases by subsystem/package
5. Include arrow legend for relationship types

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/use-case-[domain].drawio
```

---

## Generate State Machine Diagram

```
## Task: Generate State Machine Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/state-machine-analysis.md

### Requirements
1. Create state machine diagrams based on the analysis
2. Use standard UML state machine notation:
   - Rounded rectangles: States
   - Filled circle: Initial state
   - Circle with border: Final state
   - Arrows: Transitions
3. Label transitions with: trigger [guard] / action
4. Show composite states with nested regions
5. Use distinct colors for different state categories
6. Include arrow legend for transition types

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/state-machine-[entity].drawio
```

---

## Generate Infrastructure Diagram

```
## Task: Generate Infrastructure Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/infrastructure-analysis.md

### Requirements
1. Create an infrastructure diagram based on the analysis
2. Use official cloud provider shapes (AWS, Azure, GCP)
3. Show proper network hierarchy (Region → VPC → Subnet)
4. Include all compute, storage, and integration services
5. Apply protocol-based arrow colors (HTTP, SQL, Cache, Queue)
6. Include arrow legend for all protocols
7. Show security boundaries and access patterns

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/infrastructure.drawio
```

---

## Generate C4 Model Diagrams

```
## Task: Generate C4 Model Diagrams

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/c4-model-analysis.md

### Requirements
Generate THREE diagrams:

#### 1. Context Diagram (Level 1)
- Show the system as a single box
- Include all actors (persons)
- Include all external systems
- Show relationships with descriptions
- Save to: [YOUR_PROJECT_PATH]/docs/diagrams/c4-1-context.drawio

#### 2. Container Diagram (Level 2)
- Show all containers within the system boundary
- Include technology labels
- Show container-to-container relationships
- Include external systems from context
- Save to: [YOUR_PROJECT_PATH]/docs/diagrams/c4-2-container.drawio

#### 3. Component Diagram (Level 3)
- Create one diagram per major container
- Show all components within container boundary
- Show component relationships
- Save to: [YOUR_PROJECT_PATH]/docs/diagrams/c4-3-component-[container-name].drawio

### Common Requirements
- Follow C4 Model color conventions
- Include relationship descriptions on arrows
- Add arrow legend for relationship types
```

---

## Generate Security Diagram

```
## Task: Generate Security Architecture Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/security-analysis.md

### Requirements
1. Create security architecture diagram based on the analysis
2. Show trust zones/boundaries with colored regions
3. Mark entry points and attack surfaces
4. Indicate security controls at each boundary:
   - Authentication points
   - Authorization checks
   - Encryption zones
5. Show data flow with sensitivity levels
6. Use distinct colors for:
   - Public zone (red/orange)
   - DMZ (yellow)
   - Private zone (green)
   - Sensitive data zone (blue)
7. Include arrow legend for data flow types
8. Mark compliance-relevant boundaries

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/security-architecture.drawio
```

---

## Generate Dependency Diagram

```
## Task: Generate Dependency Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/dependency-analysis.md

### Requirements
1. Create module dependency diagram based on the analysis
2. Show internal modules as rectangles
3. Use arrows to show dependency direction (depends on)
4. Highlight circular dependencies in red
5. Group modules by layer/domain
6. Use line thickness to indicate coupling strength
7. Color code by module type:
   - Core: Blue
   - Infrastructure: Gray
   - External: Orange
8. Include arrow legend for dependency types

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/module-dependencies.drawio
```

---

## Generate DDD Diagram

```
## Task: Generate Domain-Driven Design Diagram

Use diagram standards from: [CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]

### Input Analysis
Read and use: [YOUR_PROJECT_PATH]/docs/analysis/ddd-analysis.md

### Requirements
1. Create a DDD context map diagram
2. Show all bounded contexts as containers
3. Include context relationships with pattern labels:
   - Customer-Supplier
   - Conformist
   - Anticorruption Layer
   - Shared Kernel
   - Partnership
4. Show key aggregates within each context
5. Include domain events flowing between contexts
6. Use distinct colors for Core, Supporting, Generic subdomains
7. Include arrow legend for relationship patterns

### Output
Save diagram to: [YOUR_PROJECT_PATH]/docs/diagrams/ddd-context-map.drawio
```

---

# Output Templates

These templates show the expected structure for analysis output files.

> **IMPORTANT:** The README.md file is **ALWAYS REQUIRED** as the main entry point and project summary. It must be created for every project.

---

## README (Project Summary) Output Template

**This file is REQUIRED for every project analysis.**

The README.md serves as:
- Main entry point for the analysis documentation
- Project overview with key metrics
- Technology stack summary
- Links to all analysis documents
- Links to all diagram artifacts
- Quick start guide

**Output Path**: `[OUTPUT_PATH]/analysis/README.md`

```markdown
# Project Analysis: [Project Name]

## Project Overview

| Attribute | Value |
|-----------|-------|
| **Project Name** | [Name] |
| **Project Type** | [Type: Web API, Monorepo, Microservices, etc.] |
| **Primary Language** | [Language and version] |
| **Framework** | [Main framework(s)] |
| **Database** | [Database type(s) or "N/A"] |
| **Build Tool** | [Build tool/package manager] |
| **Architecture** | [Architecture pattern] |

---

## Technology Stack

### Core Technologies

| Category | Technology | Version | Purpose |
|----------|------------|---------|---------|
| Language | [Language] | [Version] | [Purpose] |
| Framework | [Framework] | [Version] | [Purpose] |
| Database | [Database] | [Version] | [Purpose] |
| ... | ... | ... | ... |

### Key Libraries/Dependencies

| Library | Version | Purpose |
|---------|---------|---------|
| [Library] | [Version] | [Purpose] |
| ... | ... | ... |

---

## Framework Patterns Demonstrated

| Pattern | Implementation | Purpose |
|---------|----------------|---------|
| [Pattern Name] | [How implemented] | [Why used] |
| ... | ... | ... |

---

## Business Target

| Aspect | Description |
|--------|-------------|
| **Purpose** | [Main purpose of the application] |
| **Domain** | [Business domain] |
| **Use Case** | [Primary use cases] |
| **Complexity** | [Simple/Medium/Complex] |

---

## Module/Package Summary

| Module | Location | Type | Description |
|--------|----------|------|-------------|
| [Module Name] | [Path] | [Type] | [Description] |
| ... | ... | ... | ... |

---

## Analysis Documents

| Document | Description |
|----------|-------------|
| [architecture-analysis.md](./architecture-analysis.md) | System architecture, layers, components |
| [data-models-analysis.md](./data-models-analysis.md) | Entity definitions, relationships |
| [data-flow-analysis.md](./data-flow-analysis.md) | Data movement through system |
| ... | ... |

> **Note:** Only include links to analysis documents that were actually created.

---

## Diagram Artifacts

| Diagram | Description |
|---------|-------------|
| [architecture-overview.drawio](../diagrams/architecture-overview.drawio) | High-level architecture |
| [entity-erd.drawio](../diagrams/entity-erd.drawio) | Database relationships |
| [c4-1-context.drawio](../diagrams/c4-1-context.drawio) | C4 Context diagram |
| ... | ... |

> **Note:** Only include links to diagrams that were actually created.

---

## Quick Start

### Prerequisites
- [Prerequisite 1]
- [Prerequisite 2]

### Installation/Setup
\`\`\`bash
# Commands to set up the project
[command 1]
[command 2]
\`\`\`

### Running the Application
\`\`\`bash
# Commands to run
[command]
\`\`\`

---

## Sample Code Snippets

### [Feature/Pattern Name]
\`\`\`[language]
// Code example
\`\`\`

---

## Configuration

### [Config File Name]
\`\`\`[format]
# Configuration example
\`\`\`

---

## Key Takeaways

| Topic | Lesson |
|-------|--------|
| **[Topic 1]** | [Key insight or pattern] |
| **[Topic 2]** | [Key insight or pattern] |
| ... | ... |
```

---

## Architecture Analysis Output Template

```markdown
# Architecture Analysis: [Project Name]

## Overview
- **Project**: [Name]
- **Type**: [Web API / SPA / CLI / etc.]
- **Language**: [Primary language]
- **Framework**: [Main framework]
- **Generated**: [Date]

## Architecture Pattern
- **Pattern**: [MVC / Clean Architecture / Hexagonal / etc.]
- **Layers**:
  1. **Presentation**: [Description]
  2. **Application**: [Description]
  3. **Domain**: [Description]
  4. **Infrastructure**: [Description]

## Entry Points

### API Endpoints
| Method | Path | Description | Handler |
|--------|------|-------------|---------|
| GET | /api/users | List users | UserController.list |
| POST | /api/users | Create user | UserController.create |

### Background Jobs
| Job | Schedule | Description |
|-----|----------|-------------|
| CleanupJob | Daily 2AM | Remove expired sessions |

## Components

### [Component Name]
- **Path**: `src/components/[name]`
- **Purpose**: [Description]
- **Dependencies**: [List]
- **Key Classes**:
  - `ClassName`: [Purpose]

## External Integrations

| Service | Type | Purpose | Config Location |
|---------|------|---------|-----------------|
| Stripe | Payment | Process payments | `config/stripe.ts` |
| SendGrid | Email | Send notifications | `config/email.ts` |

## Cross-Cutting Concerns

### Authentication
- **Method**: [JWT / Session / OAuth]
- **Provider**: [Implementation details]

### Logging
- **Library**: [winston / pino / etc.]
- **Levels**: [Configured levels]

### Caching
- **Strategy**: [Redis / In-memory / etc.]
- **Cached Items**: [What is cached]
```

---

## Data Models Output Template

```markdown
# Data Models Analysis: [Project Name]

## Database Overview
- **Database**: [PostgreSQL / MySQL / MongoDB]
- **ORM**: [TypeORM / Prisma / Mongoose]
- **Connection**: `[config file path]`

## Entities

### [Entity Name] (`table_name`)
**Description**: [Purpose of this entity]

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PK, NOT NULL | Primary identifier |
| name | VARCHAR(255) | NOT NULL | Display name |
| email | VARCHAR(255) | UNIQUE, NOT NULL | User email |
| status | ENUM | DEFAULT 'active' | Account status |
| created_at | TIMESTAMP | NOT NULL | Creation time |

**Indexes**:
- `idx_users_email` (email) - UNIQUE
- `idx_users_status` (status)

---

### [Next Entity]
...

## Relationships

| From | To | Type | FK Column | Description |
|------|-----|------|-----------|-------------|
| users | orders | 1:N | user_id | User places orders |
| orders | order_items | 1:N | order_id | Order contains items |
| products | order_items | 1:N | product_id | Product in order |

## Enums

### OrderStatus
- `pending` - Awaiting payment
- `paid` - Payment received
- `shipped` - In transit
- `delivered` - Completed
- `cancelled` - Cancelled

## Entity Groups

### Core Entities
- users
- accounts

### Transactional
- orders
- order_items
- payments

### Reference Data
- categories
- countries
```

---

## Data Flow Output Template

```markdown
# Data Flow Analysis: [Project Name]

## User Flows

### [Flow Name]: User Registration
**Trigger**: User submits registration form

```
[User]
   │ POST /api/register {email, password, name}
   ▼
[API Gateway]
   │ Validate request format
   ▼
[AuthController]
   │ Validate input, hash password
   ▼
[UserService]
   │ Check email uniqueness
   │ Create user record
   ▼
[UserRepository]
   │ INSERT INTO users
   ▼
[PostgreSQL]
   │ Store user data
   ▼
[EmailService]
   │ Queue verification email
   ▼
[Response] → 201 Created {userId, message}
```

**Data Transformations**:
1. Password → bcrypt hash at AuthController
2. User DTO → User Entity at UserService

---

### [Next Flow]
...

## Event Flows

### OrderPlaced Event
**Producer**: OrderService.createOrder()
**Payload**:
```json
{
  "eventType": "OrderPlaced",
  "orderId": "uuid",
  "customerId": "uuid",
  "items": [...],
  "total": 99.99
}
```

**Consumers**:
| Consumer | Action |
|----------|--------|
| InventoryService | Reserve stock |
| NotificationService | Send confirmation email |
| AnalyticsService | Track conversion |
```

---

## Flowchart Output Template

```markdown
# Flowchart Analysis: [Project Name]

## Overview
- **Project**: [Name]
- **Analyzed**: [Date]
- **Total Processes**: [Count]

## Business Processes

### [Process Name]: Order Checkout
**Description**: Customer completes purchase from cart to confirmation
**Trigger**: User clicks "Checkout" button
**Actors**: Customer, Payment System, Inventory System

#### Process Flow
```
[Start: Cart Page]
    │
    ▼
[Display Order Summary]
    │
    ▼
[Enter Shipping Address]
    │
    ▼
◇─── Is address valid? ───◇
│                          │
│ No                       │ Yes
▼                          ▼
[Show Error]          [Select Shipping Method]
    │                      │
    └──────────────────────┘
                           │
                           ▼
                    [Enter Payment Info]
                           │
                           ▼
                    ◇─── Process Payment ───◇
                    │                        │
                    │ Failed                 │ Success
                    ▼                        ▼
              [Show Error]            [Create Order]
                    │                        │
                    └────────────────────────┤
                                             ▼
                                    [Send Confirmation]
                                             │
                                             ▼
                                    [End: Order Complete]
```

#### Decision Points
| Decision | Condition | Yes Path | No Path |
|----------|-----------|----------|---------|
| Is address valid? | Address passes validation | Continue to shipping | Show validation errors |
| Process Payment | Payment gateway returns success | Create order | Show payment error |

#### Error Paths
| Error | Trigger | Recovery Action |
|-------|---------|-----------------|
| Invalid address | Validation fails | Return to address form |
| Payment failed | Gateway rejection | Return to payment form |
| Inventory unavailable | Stock check fails | Remove item, notify user |

---

### [Next Process]
...

## User Journeys

### Journey: First-Time Buyer
**Persona**: New customer discovering the platform
**Goal**: Complete first purchase successfully

| Step | Action | Decision Point | Success | Failure |
|------|--------|----------------|---------|---------|
| 1 | Browse products | - | View product | Exit |
| 2 | Add to cart | - | Cart updated | Show error |
| 3 | Create account | Has account? | Login | Register |
| 4 | Checkout | - | Payment page | Cart issues |
| 5 | Payment | Valid payment? | Confirmation | Retry |

## State Transitions

### Order States
```
[Draft] ──create──▶ [Pending]
                        │
            ┌───────────┴───────────┐
            │ payment_received      │ cancelled
            ▼                       ▼
       [Confirmed]            [Cancelled]
            │
            │ shipped
            ▼
       [Shipped]
            │
            │ delivered
            ▼
       [Delivered]
```

| From State | To State | Trigger | Actions |
|------------|----------|---------|---------|
| Draft | Pending | User submits order | Validate items, reserve stock |
| Pending | Confirmed | Payment received | Update inventory, notify user |
| Pending | Cancelled | User cancels / timeout | Release stock, refund if paid |
| Confirmed | Shipped | Fulfillment complete | Generate tracking, notify user |
| Shipped | Delivered | Carrier confirms | Close order, request review |

## Approval Workflows

### Refund Request Workflow
**Actors**: Customer, Support Agent, Finance Team

| Step | Actor | Action | Next Step |
|------|-------|--------|-----------|
| 1 | Customer | Submit refund request | 2 |
| 2 | Support Agent | Review request | 3 (approve) or 4 (reject) |
| 3 | Finance Team | Process refund | 5 |
| 4 | Support Agent | Notify rejection | End |
| 5 | System | Send confirmation | End |
```

---

## Sequence Diagram Output Template

```markdown
# Sequence Diagram Analysis: [Project Name]

## Overview
- **Project**: [Name]
- **Analyzed**: [Date]
- **Total Sequences**: [Count]

## API Sequences

### Sequence: User Login
**Trigger**: User submits login form
**Participants**: Client, API Gateway, AuthService, UserRepository, Database, TokenService

| # | From | To | Message | Return | Notes |
|---|------|-----|---------|--------|-------|
| 1 | Client | API Gateway | POST /api/auth/login | - | {email, password} |
| 2 | API Gateway | AuthService | authenticate(email, password) | - | |
| 3 | AuthService | UserRepository | findByEmail(email) | User | |
| 4 | UserRepository | Database | SELECT * FROM users WHERE email = ? | Row | |
| 5 | AuthService | AuthService | verifyPassword(password, hash) | boolean | Self-call |
| 6 | AuthService | TokenService | generateToken(userId) | JWT | |
| 7 | AuthService | API Gateway | - | AuthResponse | {token, user} |
| 8 | API Gateway | Client | - | 200 OK | {token, user} |

**Alternative Flow - Invalid Credentials**:
| # | From | To | Message | Return |
|---|------|-----|---------|--------|
| 5a | AuthService | API Gateway | - | AuthError | Invalid credentials |
| 6a | API Gateway | Client | - | 401 | Error response |

---

### Sequence: [Next Sequence]
...

## Service Sequences

### Sequence: Order Processing
**Trigger**: Order placed event
**Participants**: OrderService, InventoryService, PaymentService, NotificationService

| # | From | To | Message | Async | Notes |
|---|------|-----|---------|-------|-------|
| 1 | OrderService | InventoryService | reserveStock(items) | No | Sync call |
| 2 | OrderService | PaymentService | processPayment(amount) | No | Sync call |
| 3 | OrderService | NotificationService | sendConfirmation(order) | Yes | Fire & forget |

## Message Patterns

### Synchronous Calls
- Request-response pattern
- Caller waits for response
- Used for: Data queries, validations

### Asynchronous Calls
- Fire-and-forget pattern
- Caller continues immediately
- Used for: Notifications, logging, analytics
```

---

## Use Case Output Template

```markdown
# Use Case Analysis: [Project Name]

## Overview
- **Project**: [Name]
- **Domain**: [Business domain]
- **Analyzed**: [Date]
- **Total Use Cases**: [Count]

## Actors

| Actor | Type | Description | Primary Use Cases |
|-------|------|-------------|-------------------|
| Customer | Person | End user purchasing products | Browse, Purchase, Review |
| Admin | Person | System administrator | Manage Users, View Reports |
| Payment System | External System | Payment gateway | Process Payments |
| Scheduler | System | Cron/timer service | Run Scheduled Jobs |

## Use Cases

### UC-001: Place Order
**Actor(s)**: Customer (Primary), Payment System (Secondary)
**Description**: Customer completes purchase of items in cart

**Preconditions**:
- Customer is authenticated
- Cart contains at least one item
- Items are in stock

**Postconditions**:
- Order is created with status "Pending"
- Inventory is reserved
- Confirmation email is sent

**Main Flow**:
| Step | Actor | Action | System Response |
|------|-------|--------|-----------------|
| 1 | Customer | Clicks "Checkout" | Display order summary |
| 2 | Customer | Confirms shipping address | Validate address |
| 3 | Customer | Selects shipping method | Calculate total |
| 4 | Customer | Enters payment details | - |
| 5 | System | Validates payment with Payment System | - |
| 6 | Payment System | Returns authorization | - |
| 7 | System | Creates order | Display confirmation |

**Alternative Flows**:
- **3a**: Invalid address → Show error, return to step 2
- **5a**: Payment declined → Show error, return to step 4

**Exception Flows**:
- **E1**: Item out of stock during checkout → Remove item, notify user
- **E2**: System timeout → Save cart, show retry option

---

### UC-002: [Next Use Case]
...

## Use Case Relationships

### Include Relationships
| Base Use Case | Included Use Case | Description |
|---------------|-------------------|-------------|
| Place Order | Validate Payment | Always validates payment |
| Place Order | Send Notification | Always sends confirmation |

### Extend Relationships
| Base Use Case | Extending Use Case | Condition |
|---------------|---------------------|-----------|
| Place Order | Apply Coupon | User has coupon code |
| View Product | Add to Wishlist | User is authenticated |

## Actor-Use Case Matrix

| Use Case | Customer | Admin | Guest | System |
|----------|----------|-------|-------|--------|
| Browse Products | ✓ | ✓ | ✓ | - |
| Place Order | ✓ | - | - | - |
| Manage Users | - | ✓ | - | - |
| Send Notifications | - | - | - | ✓ |
```

---

## State Machine Output Template

```markdown
# State Machine Analysis: [Project Name]

## Overview
- **Project**: [Name]
- **Analyzed**: [Date]
- **Stateful Entities**: [Count]

## State Machines

### Order State Machine
**Entity**: Order
**State Field**: `order.status` (Enum)
**Initial State**: Draft

#### States
| State | Description | Entry Action | Exit Action |
|-------|-------------|--------------|-------------|
| Draft | Order being created | Initialize order | Validate items |
| Pending | Awaiting payment | Reserve inventory | - |
| Confirmed | Payment received | Update inventory | - |
| Processing | Being prepared | Assign to fulfillment | - |
| Shipped | In transit | Generate tracking | - |
| Delivered | Completed | Close order | - |
| Cancelled | Cancelled by user/system | Release inventory | - |
| Refunded | Payment returned | Process refund | - |

#### Transitions
| From | To | Trigger | Guard | Action |
|------|-----|---------|-------|--------|
| Draft | Pending | submit() | items.length > 0 | createOrder() |
| Pending | Confirmed | paymentReceived | payment.valid | confirmOrder() |
| Pending | Cancelled | cancel() | - | releaseInventory() |
| Pending | Cancelled | timeout | time > 30min | autoCancel() |
| Confirmed | Processing | startProcessing() | - | notifyWarehouse() |
| Processing | Shipped | ship() | trackingNumber set | sendShipmentEmail() |
| Shipped | Delivered | confirmDelivery() | - | requestReview() |
| Confirmed | Refunded | requestRefund() | within 30 days | processRefund() |

#### State Diagram
```
    ┌─────────────────────────────────────────────────────┐
    │                                                     │
    ▼                                                     │
[Draft] ──submit──▶ [Pending] ──payment──▶ [Confirmed]    │
                        │                      │          │
                        │ cancel/timeout       │ start    │
                        ▼                      ▼          │
                   [Cancelled]          [Processing]      │
                                              │           │
                                              │ ship      │
                                              ▼           │
                        ┌──────────────── [Shipped]       │
                        │                     │           │
                        │ refund              │ deliver   │
                        ▼                     ▼           │
                   [Refunded] ◀─refund── [Delivered] ─────┘
```

---

### User Account State Machine
**Entity**: User
**State Field**: `user.status` (Enum)
**Initial State**: Pending

#### States
| State | Description |
|-------|-------------|
| Pending | Email not verified |
| Active | Normal active account |
| Suspended | Temporarily disabled |
| Banned | Permanently disabled |
| Deleted | Soft deleted |

#### Transitions
| From | To | Trigger | Guard |
|------|-----|---------|-------|
| Pending | Active | verifyEmail() | token.valid |
| Active | Suspended | suspend() | isAdmin |
| Suspended | Active | reactivate() | isAdmin |
| Active | Banned | ban() | isAdmin |
| Active | Deleted | deleteAccount() | isOwner |

---

## Composite States

### Order.Processing (Composite)
**Parent State**: Processing
**Child States**:
- Picking: Items being collected
- Packing: Items being packaged
- QualityCheck: Verifying order accuracy

| From | To | Trigger |
|------|-----|---------|
| Picking | Packing | itemsCollected() |
| Packing | QualityCheck | packed() |
| QualityCheck | [Exit] | approved() |
```

---

## Infrastructure Output Template

```markdown
# Infrastructure Analysis: [Project Name]

## Cloud Overview
- **Provider**: [AWS / Azure / GCP]
- **Region**: [Primary region]
- **Environment**: [Production / Staging / Dev]

## Network Architecture

### VPC: `vpc-main`
- **CIDR**: 10.0.0.0/16
- **Region**: us-east-1

### Subnets
| Subnet | CIDR | Type | AZ | Purpose |
|--------|------|------|-----|---------|
| subnet-public-1 | 10.0.1.0/24 | Public | us-east-1a | Load balancers |
| subnet-private-1 | 10.0.10.0/24 | Private | us-east-1a | Application |
| subnet-data-1 | 10.0.20.0/24 | Private | us-east-1a | Databases |

## Compute Resources

### [Resource Name]
- **Type**: ECS Fargate / EC2 / Lambda
- **Specs**: 2 vCPU, 4GB RAM
- **Scaling**: Min 2, Max 10, Target CPU 70%
- **Subnet**: subnet-private-1
- **Security Group**: sg-app-servers

## Data Storage

### [Database Name]
- **Type**: RDS PostgreSQL
- **Instance**: db.r5.large
- **Storage**: 100GB GP2
- **Multi-AZ**: Yes
- **Subnet Group**: db-subnet-group

## Security

### Security Groups
| Name | Inbound | Outbound |
|------|---------|----------|
| sg-alb | 443 from 0.0.0.0/0 | All to sg-app |
| sg-app | 8080 from sg-alb | All to sg-db |
| sg-db | 5432 from sg-app | None |

## Integration Services

### Message Queue: SQS
| Queue | Purpose | DLQ |
|-------|---------|-----|
| orders-queue | Order processing | orders-dlq |
| notifications-queue | Send notifications | notifications-dlq |
```

---

## C4 Model Output Template

```markdown
# C4 Model Analysis: [Project Name]

## Level 1: System Context

### System
- **Name**: [System Name]
- **Description**: [What the system does]

### Actors
| Actor | Type | Description |
|-------|------|-------------|
| Customer | Person | End user who purchases products |
| Admin | Person | System administrator |
| Support Agent | Person | Customer support staff |

### External Systems
| System | Description | Interaction |
|--------|-------------|-------------|
| Payment Gateway | Stripe payment processing | Send payment requests, receive confirmations |
| Email Service | SendGrid email delivery | Send transactional emails |
| Analytics | Google Analytics | Send user events |

---

## Level 2: Containers

### Containers
| Container | Technology | Description |
|-----------|------------|-------------|
| Web Application | React 18 | Single-page application for customers |
| Admin Portal | React 18 | Admin dashboard |
| API Server | Node.js / Express | REST API backend |
| Worker | Node.js | Background job processor |
| Database | PostgreSQL 14 | Primary data store |
| Cache | Redis 7 | Session and query cache |
| Message Queue | RabbitMQ | Async job queue |

### Container Relationships
| From | To | Technology | Description |
|------|-----|------------|-------------|
| Web Application | API Server | HTTPS/JSON | API calls |
| API Server | Database | TCP/SQL | Read/write data |
| API Server | Cache | TCP/Redis | Cache queries |
| API Server | Message Queue | AMQP | Queue jobs |
| Worker | Message Queue | AMQP | Process jobs |
| Worker | Database | TCP/SQL | Update data |

---

## Level 3: Components

### Container: API Server

| Component | Technology | Description |
|-----------|------------|-------------|
| Auth Module | Passport.js | Authentication & authorization |
| User Module | Express Router | User management endpoints |
| Order Module | Express Router | Order processing |
| Product Module | Express Router | Product catalog |
| Notification Service | Custom | Send notifications |

### Component Relationships
| From | To | Description |
|------|-----|-------------|
| Auth Module | User Module | Validate user identity |
| Order Module | Product Module | Get product details |
| Order Module | Notification Service | Send order confirmation |
```

---

## DDD Output Template

```markdown
# Domain-Driven Design Analysis: [Project Name]

## Domain Overview
- **Domain**: [E-commerce / Banking / Healthcare]
- **Core Problem**: [What business problem is solved]

## Subdomains

| Subdomain | Type | Description |
|-----------|------|-------------|
| Order Management | Core | Order processing and fulfillment |
| Product Catalog | Core | Product information management |
| User Management | Supporting | Customer and admin accounts |
| Payment Processing | Supporting | Handle payments |
| Notification | Generic | Send emails and notifications |
| Shipping | Generic | Delivery logistics |

---

## Bounded Contexts

### Order Context
- **Description**: Handles order lifecycle from cart to delivery
- **Team**: Order Team
- **Ubiquitous Language**:
  - **Order**: A customer's purchase request
  - **Line Item**: Single product in an order
  - **Fulfillment**: Process of preparing and shipping

### Product Context
- **Description**: Product catalog and inventory
- **Team**: Product Team
- **Ubiquitous Language**:
  - **Product**: Sellable item
  - **SKU**: Stock keeping unit
  - **Variant**: Product variation (size, color)

---

## Context Map

| Upstream | Downstream | Pattern | Description |
|----------|------------|---------|-------------|
| Product | Order | Customer-Supplier | Order requests product info |
| Order | Shipping | Customer-Supplier | Order triggers shipping |
| Payment | Order | Conformist | Order accepts payment events |
| User | Order | Shared Kernel | Shared customer identity |

---

## Aggregates

### Order Aggregate
- **Root**: Order
- **Entities**: OrderLineItem
- **Value Objects**: Money, Address, OrderStatus
- **Invariants**:
  - Order total must equal sum of line items
  - Cannot modify order after shipped
- **Domain Events**:
  - OrderPlaced
  - OrderPaid
  - OrderShipped
  - OrderCancelled

### Product Aggregate
- **Root**: Product
- **Entities**: ProductVariant
- **Value Objects**: Price, SKU, Dimensions
- **Invariants**:
  - Product must have at least one variant
  - SKU must be unique

---

## Domain Events

| Event | Aggregate | Payload | Consumers |
|-------|-----------|---------|-----------|
| OrderPlaced | Order | orderId, customerId, items, total | Inventory, Payment, Notification |
| PaymentReceived | Payment | paymentId, orderId, amount | Order |
| InventoryReserved | Inventory | orderId, items | Order |
| OrderShipped | Order | orderId, trackingNumber | Notification, Customer |

---

## Domain Services

| Service | Context | Responsibility |
|---------|---------|----------------|
| PricingService | Order | Calculate order total with discounts |
| InventoryAllocationService | Inventory | Reserve and allocate stock |
| ShippingCalculator | Shipping | Calculate shipping cost and ETA |
```

---

## Security Analysis Output Template

```markdown
# Security Analysis: [Project Name]

## Overview
- **Project**: [Name]
- **Analyzed**: [Date]
- **Risk Level**: [Low/Medium/High]

## Authentication

### Method: JWT + OAuth2
- **Provider**: Auth0 / Custom
- **Token Type**: Bearer token (JWT)
- **Token Storage**: HttpOnly cookie (web), Secure storage (mobile)
- **Token Lifetime**: Access: 15min, Refresh: 7 days
- **Refresh Mechanism**: Silent refresh via refresh token
- **MFA**: Optional TOTP, required for admin

### Implementation
| Component | Location | Purpose |
|-----------|----------|---------|
| AuthMiddleware | `src/middleware/auth.ts` | Validate JWT tokens |
| TokenService | `src/services/token.ts` | Generate/verify tokens |
| RefreshController | `src/controllers/auth.ts` | Handle token refresh |

## Authorization

### Model: Role-Based Access Control (RBAC)

### Roles & Permissions
| Role | Permissions | Description |
|------|-------------|-------------|
| super_admin | * | Full system access |
| admin | users.*, orders.*, products.* | Administrative access |
| manager | orders.read, orders.update, reports.read | Order management |
| user | own.*, products.read | Standard user |
| guest | products.read, public.* | Unauthenticated |

### Permission Enforcement
| Layer | Location | Method |
|-------|----------|--------|
| API Gateway | `src/middleware/authorize.ts` | @RequirePermission decorator |
| Service | `src/services/*.ts` | Manual permission checks |
| Database | Row-level security | Postgres RLS policies |

## Security Boundaries

### Trust Zones
| Zone | Components | Trust Level |
|------|------------|-------------|
| Public Internet | CDN, Public APIs | Untrusted |
| DMZ | Load Balancer, WAF | Semi-trusted |
| Application Zone | API Servers, Workers | Trusted |
| Data Zone | Database, Cache | Highly Trusted |
| Management Zone | Admin tools, CI/CD | Restricted |

### Entry Points
| Entry Point | Zone Boundary | Controls |
|-------------|---------------|----------|
| HTTPS :443 | Public → DMZ | WAF, Rate limiting |
| API Gateway | DMZ → App | Auth, CORS, Validation |
| Database :5432 | App → Data | VPC, Security group |
| Admin :8443 | Public → Mgmt | VPN, MFA, IP whitelist |

## Data Protection

### Sensitive Data Classification
| Data Type | Classification | Protection |
|-----------|----------------|------------|
| Passwords | Secret | bcrypt hash, never stored plain |
| PII (name, email) | Confidential | Encrypted at rest, masked in logs |
| Payment card | PCI | Tokenized via Stripe, not stored |
| Session tokens | Secret | HttpOnly, Secure, SameSite |

### Encryption
| Layer | Algorithm | Key Management |
|-------|-----------|----------------|
| At Rest | AES-256 | AWS KMS |
| In Transit | TLS 1.3 | ACM certificates |
| Application | AES-256-GCM | Vault |

## Threat Model

### Identified Threats (STRIDE)
| Threat | Category | Asset | Likelihood | Impact | Mitigation | Status |
|--------|----------|-------|------------|--------|------------|--------|
| SQL Injection | Tampering | Database | Medium | High | Parameterized queries | ✅ |
| XSS | Information Disclosure | User session | Medium | Medium | Output encoding, CSP | ✅ |
| CSRF | Elevation of Privilege | User actions | Low | Medium | CSRF tokens, SameSite | ✅ |
| Brute Force | Spoofing | User accounts | High | Medium | Rate limiting, lockout | ✅ |
| Session Hijacking | Spoofing | Sessions | Low | High | Secure cookies, rotation | ✅ |

## Security Controls

### Input Validation
| Control | Implementation | Location |
|---------|----------------|----------|
| Schema validation | Zod/Joi | API controllers |
| SQL injection | Parameterized queries | All repositories |
| Path traversal | Path sanitization | File upload service |

### Headers
```
Strict-Transport-Security: max-age=31536000; includeSubDomains
Content-Security-Policy: default-src 'self'; script-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
```

### Rate Limiting
| Endpoint | Limit | Window | Action |
|----------|-------|--------|--------|
| /api/auth/login | 5 | 15 min | Block IP |
| /api/* | 100 | 1 min | 429 response |
| /api/public/* | 1000 | 1 min | 429 response |

## Secrets Management

### Secret Types
| Secret | Storage | Rotation |
|--------|---------|----------|
| Database password | AWS Secrets Manager | 90 days |
| API keys | Environment variables | Manual |
| JWT signing key | AWS Secrets Manager | 30 days |
| Encryption keys | AWS KMS | Annual |

## Compliance

### GDPR
| Requirement | Implementation |
|-------------|----------------|
| Right to access | GET /api/users/me/data |
| Right to deletion | DELETE /api/users/me |
| Data portability | GET /api/users/me/export |
| Consent management | Consent service + audit log |
```

---

## API Contract Output Template

```markdown
# API Contract Analysis: [Project Name]

## Overview
- **API Type**: REST
- **Base URL**: `https://api.example.com`
- **Version**: v1 (current), v0 (deprecated)
- **Version Strategy**: URL path (`/api/v1/...`)
- **Spec Location**: `docs/openapi.yaml`

## Authentication

### Bearer Token (JWT)
```
Authorization: Bearer <token>
```

### API Key (for service-to-service)
```
X-API-Key: <api-key>
```

## Endpoints

### Authentication
| Method | Path | Summary | Auth | Rate Limit |
|--------|------|---------|------|------------|
| POST | /api/v1/auth/register | Register new user | None | 5/15min |
| POST | /api/v1/auth/login | User login | None | 5/15min |
| POST | /api/v1/auth/refresh | Refresh token | Refresh Token | 10/min |
| POST | /api/v1/auth/logout | User logout | Bearer | 10/min |

### Users
| Method | Path | Summary | Auth | Permissions |
|--------|------|---------|------|-------------|
| GET | /api/v1/users | List users | Bearer | users.read |
| POST | /api/v1/users | Create user | Bearer | users.create |
| GET | /api/v1/users/:id | Get user | Bearer | users.read |
| PUT | /api/v1/users/:id | Update user | Bearer | users.update |
| DELETE | /api/v1/users/:id | Delete user | Bearer | users.delete |

### Orders
| Method | Path | Summary | Auth | Permissions |
|--------|------|---------|------|-------------|
| GET | /api/v1/orders | List orders | Bearer | orders.read |
| POST | /api/v1/orders | Create order | Bearer | orders.create |
| GET | /api/v1/orders/:id | Get order | Bearer | orders.read |
| PATCH | /api/v1/orders/:id | Update order | Bearer | orders.update |
| POST | /api/v1/orders/:id/cancel | Cancel order | Bearer | orders.update |

## Schemas

### User
```json
{
  "id": "uuid",
  "email": "string (email)",
  "name": "string",
  "role": "user | admin",
  "status": "active | suspended",
  "createdAt": "datetime",
  "updatedAt": "datetime"
}
```

### CreateUserRequest
```json
{
  "email": "string (required, email)",
  "password": "string (required, min 8)",
  "name": "string (required)"
}
```

### Order
```json
{
  "id": "uuid",
  "userId": "uuid",
  "status": "draft | pending | confirmed | shipped | delivered | cancelled",
  "items": "OrderItem[]",
  "total": "number",
  "shippingAddress": "Address",
  "createdAt": "datetime",
  "updatedAt": "datetime"
}
```

## Error Responses

### Standard Error Format
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ],
    "requestId": "uuid"
  }
}
```

### Error Codes
| Code | HTTP | Description |
|------|------|-------------|
| VALIDATION_ERROR | 400 | Request validation failed |
| INVALID_CREDENTIALS | 401 | Login failed |
| UNAUTHORIZED | 401 | Token invalid or expired |
| FORBIDDEN | 403 | Insufficient permissions |
| NOT_FOUND | 404 | Resource not found |
| CONFLICT | 409 | Resource already exists |
| RATE_LIMITED | 429 | Too many requests |
| INTERNAL_ERROR | 500 | Server error |

## Pagination

### Request Parameters
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| page | integer | 1 | Page number |
| limit | integer | 20 | Items per page (max 100) |
| sort | string | createdAt | Sort field |
| order | string | desc | Sort order (asc/desc) |

### Response Format
```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "totalPages": 8
  }
}
```

## Webhooks

### Events
| Event | Trigger | Payload |
|-------|---------|---------|
| order.created | New order placed | Order object |
| order.updated | Order status changed | Order object |
| payment.received | Payment confirmed | Payment object |
| user.created | New user registered | User object (sanitized) |

### Delivery
- **Method**: POST
- **Content-Type**: application/json
- **Signature**: HMAC-SHA256 in `X-Webhook-Signature`
- **Retry Policy**: 3 attempts, exponential backoff
```

---

## Dependency Analysis Output Template

```markdown
# Dependency Analysis: [Project Name]

## Overview
- **Project**: [Name]
- **Package Manager**: npm / yarn / pnpm
- **Analyzed**: [Date]
- **Total Dependencies**: 156

## Summary

| Category | Count |
|----------|-------|
| Direct (Production) | 32 |
| Direct (Dev) | 24 |
| Transitive | 100 |
| Outdated | 8 |
| Vulnerabilities | 2 |

## Production Dependencies

### Framework & Core
| Package | Version | Latest | Purpose | License |
|---------|---------|--------|---------|---------|
| express | ^4.18.2 | 4.18.2 | Web framework | MIT |
| typescript | ^5.2.0 | 5.3.2 | Type checking | Apache-2.0 |
| prisma | ^5.6.0 | 5.7.0 | ORM | Apache-2.0 |

### Database
| Package | Version | Latest | Purpose | License |
|---------|---------|--------|---------|---------|
| @prisma/client | ^5.6.0 | 5.7.0 | Database client | Apache-2.0 |
| redis | ^4.6.0 | 4.6.10 | Cache client | MIT |

### Authentication
| Package | Version | Latest | Purpose | License |
|---------|---------|--------|---------|---------|
| jsonwebtoken | ^9.0.0 | 9.0.2 | JWT handling | MIT |
| bcryptjs | ^2.4.3 | 2.4.3 | Password hashing | MIT |
| passport | ^0.6.0 | 0.7.0 | Auth middleware | MIT |

### Validation
| Package | Version | Latest | Purpose | License |
|---------|---------|--------|---------|---------|
| zod | ^3.22.0 | 3.22.4 | Schema validation | MIT |
| class-validator | ^0.14.0 | 0.14.0 | Decorator validation | MIT |

## Dev Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| jest | ^29.7.0 | Testing framework |
| @types/node | ^20.9.0 | Node.js types |
| eslint | ^8.54.0 | Linting |
| prettier | ^3.1.0 | Code formatting |

## Module Dependencies (Internal)

### Core Modules
| Module | Path | Depends On | Used By |
|--------|------|------------|---------|
| UserService | src/services/user | UserRepository, AuthService | OrderService, AdminController |
| OrderService | src/services/order | UserService, ProductService, PaymentService | OrderController, WebhookHandler |
| AuthService | src/services/auth | UserRepository, TokenService | AuthController, UserService |

### Dependency Graph
```
                    ┌─────────────────┐
                    │  AuthController │
                    └────────┬────────┘
                             │
                             ▼
┌──────────────┐    ┌─────────────────┐    ┌──────────────┐
│ UserService  │◀───│   AuthService   │───▶│ TokenService │
└──────┬───────┘    └─────────────────┘    └──────────────┘
       │
       ▼
┌──────────────┐
│UserRepository│
└──────────────┘
```

## Circular Dependencies

| Cycle | Modules | Severity | Resolution |
|-------|---------|----------|------------|
| 1 | UserService ↔ OrderService | Medium | Extract shared logic to CustomerService |

## Outdated Dependencies

| Package | Current | Latest | Breaking | Recommended Action |
|---------|---------|--------|----------|-------------------|
| passport | 0.6.0 | 0.7.0 | Yes | Update with testing |
| webpack | 4.46.0 | 5.89.0 | Yes | Plan major upgrade |
| lodash | 4.17.20 | 4.17.21 | No | Update immediately |

## Security Vulnerabilities

| Package | Severity | CVE | Description | Fix |
|---------|----------|-----|-------------|-----|
| axios | High | CVE-2023-XXXXX | SSRF vulnerability | Update to 1.6.0+ |
| semver | Moderate | CVE-2022-XXXXX | ReDoS vulnerability | Update to 7.5.2+ |

## License Compliance

| License | Count | Packages | Commercial Use |
|---------|-------|----------|----------------|
| MIT | 120 | express, lodash, ... | ✅ Allowed |
| Apache-2.0 | 25 | typescript, prisma, ... | ✅ Allowed |
| ISC | 8 | glob, rimraf, ... | ✅ Allowed |
| BSD-3-Clause | 3 | source-map, ... | ✅ Allowed |

## Recommendations

1. **Update Critical**: axios, semver (security vulnerabilities)
2. **Plan Upgrade**: passport, webpack (breaking changes)
3. **Resolve Circular**: UserService ↔ OrderService
4. **Review Large**: Consider tree-shaking lodash
```

---

## Complete Workflow Example

### Step 1: Run Analysis
> **Note:** Only generate analysis files that are relevant to your project. Skip types that don't apply.

```
Analyze project at: D:\Projects\my-ecommerce-app

Generate applicable analysis documents (skip if not relevant):
1. Architecture analysis → docs/analysis/architecture-analysis.md
2. Data models analysis → docs/analysis/data-models-analysis.md
3. Data flow analysis → docs/analysis/data-flow-analysis.md
4. Flowchart analysis → docs/analysis/flowchart-analysis.md
5. Sequence analysis → docs/analysis/sequence-analysis.md
6. Use case analysis → docs/analysis/use-case-analysis.md
7. State machine analysis → docs/analysis/state-machine-analysis.md
8. Infrastructure analysis → docs/analysis/infrastructure-analysis.md
9. C4 model analysis → docs/analysis/c4-model-analysis.md
10. DDD analysis → docs/analysis/ddd-analysis.md
11. Security analysis → docs/analysis/security-analysis.md
12. API contract analysis → docs/analysis/api-contract-analysis.md
13. Dependency analysis → docs/analysis/dependency-analysis.md
```

### Step 2: Review & Correct
- Open each `.md` file
- Verify accuracy
- Add missing details
- Correct any errors
- Delete any analysis files that are mostly empty or not applicable

### Step 3: Generate Diagrams
```
Use diagram standards from: D:\Standards\PROMPT-DIAGRAM-FORMAT.md

Generate diagrams from analysis:
1. Architecture diagram from architecture-analysis.md
2. ERD diagram from data-models-analysis.md
3. Data flow diagram from data-flow-analysis.md
4. Flowchart diagrams from flowchart-analysis.md
5. Sequence diagrams from sequence-analysis.md
6. Use case diagram from use-case-analysis.md
7. State machine diagrams from state-machine-analysis.md
8. Infrastructure diagram from infrastructure-analysis.md
9. C4 diagrams from c4-model-analysis.md
10. DDD context map from ddd-analysis.md
11. Security architecture diagram from security-analysis.md
12. Module dependency diagram from dependency-analysis.md

Save all diagrams to: docs/diagrams/
```

---

## Parameters Reference

When using these templates, replace:

| Parameter | Description | Example |
|-----------|-------------|---------|
| `[YOUR_PROJECT_PATH]` | Path to project being analyzed | `D:\Projects\my-app` |
| `[CLAUDE_DIAGRAMS_STANDARD_FORMAT_PATH]` | Path to diagram standards | `D:\Standards\CLAUDE-DIAGRAMS-STANDARD-FORMAT.md` |
| `[Date]` | Current date | `2024-01-15` |
