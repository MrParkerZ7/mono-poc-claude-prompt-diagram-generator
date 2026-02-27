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

---

## Table of Contents

### Analysis Prompts (Step 1)
- [Architecture Analysis](#1-architecture-analysis-prompt)
- [Data Models / ERD Analysis](#2-data-models--erd-analysis-prompt)
- [Data Flow Analysis](#3-data-flow-analysis-prompt)
- [Infrastructure Analysis](#4-infrastructure-analysis-prompt)
- [C4 Model Analysis](#5-c4-model-analysis-prompt)
- [Domain-Driven Design Analysis](#6-domain-driven-design-analysis-prompt)

### Diagram Generation Prompts (Step 2)
- [Generate Diagrams from Analysis](#step-2-generate-diagrams-from-analysis)

### Output Templates
- [Architecture Analysis Output Template](#architecture-analysis-output-template)
- [Data Models Output Template](#data-models-output-template)
- [Data Flow Output Template](#data-flow-output-template)
- [Infrastructure Output Template](#infrastructure-output-template)
- [C4 Model Output Template](#c4-model-output-template)
- [DDD Output Template](#ddd-output-template)

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

## 4. Infrastructure Analysis Prompt

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

## 5. C4 Model Analysis Prompt

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

## 6. Domain-Driven Design Analysis Prompt

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

## Complete Workflow Example

### Step 1: Run Analysis
```
Analyze project at: D:\Projects\my-ecommerce-app

Generate all analysis documents:
1. Architecture analysis → docs/analysis/architecture-analysis.md
2. Data models analysis → docs/analysis/data-models-analysis.md
3. Data flow analysis → docs/analysis/data-flow-analysis.md
4. Infrastructure analysis → docs/analysis/infrastructure-analysis.md
5. C4 model analysis → docs/analysis/c4-model-analysis.md
6. DDD analysis → docs/analysis/ddd-analysis.md
```

### Step 2: Review & Correct
- Open each `.md` file
- Verify accuracy
- Add missing details
- Correct any errors

### Step 3: Generate Diagrams
```
Use diagram standards from: D:\Standards\CLAUDE-DIAGRAMS-STANDARD-FORMAT.md

Generate diagrams from analysis:
1. Architecture diagram from architecture-analysis.md
2. ERD diagram from data-models-analysis.md
3. Data flow diagram from data-flow-analysis.md
4. Infrastructure diagram from infrastructure-analysis.md
5. C4 diagrams from c4-model-analysis.md
6. DDD context map from ddd-analysis.md

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
