# DrawIO CLAUDE.md Research

Project-specific instructions (CLAUDE.md) for Claude Code to generate DrawIO architecture diagrams.

## Purpose

This folder contains research and reference materials for creating `CLAUDE.md` files that enable Claude Code to generate professional DrawIO diagrams.

## Contents

### CLAUDE.md

The main reference file containing:
- DrawIO XML structure and syntax
- Shape references for all major platforms (AWS, Azure, GCP, Kubernetes)
- Network and infrastructure shapes
- C4 Model architecture diagrams
- Software architecture patterns (DDD, microservices, UML)
- Database ERD (Entity-Relationship Diagrams)
- Data workflow and ETL shapes
- BPMN, flowcharts, sequence diagrams
- Arrow color standards (protocol-based)
- Style guidelines, colors, and best practices

### Sample Diagrams

Reference diagrams demonstrating the CLAUDE.md guidelines:

#### Cloud Architecture

| Diagram | Preview |
|---------|---------|
| **AWS ECS Architecture**<br>`sample-architecture-aws-ecs.drawio`<br>VPC, subnets, Fargate, RDS, Redis, SQS | ![AWS ECS](sample-architecture-aws-ecs.png) |
| **Azure WebApp Architecture**<br>`sample-architecture-azure-webapp.drawio`<br>App Service, Functions, SQL, Cosmos DB, Redis | ![Azure WebApp](sample-architecture-azure-webapp.png) |
| **AWS + K8s Insurance Portal**<br>`sample-architecture-aws-portal-k8s-insurance.drawio`<br>Hybrid AWS cloud + On-prem K8s | ![AWS K8s Insurance](sample-architecture-aws-portal-k8s-insurance.png) |
| **On-Prem K8s CoBroker Portal**<br>`sample-architecture-insurance-cobroker-portal-k8s-onprem.drawio`<br>Full K8s on-premises with data layer | ![K8s OnPrem](sample-architecture-insurance-cobroker-portal-k8s-onprem.png) |

#### Software Architecture

| Diagram | Preview |
|---------|---------|
| **C4 Model - Core Banking**<br>`sample-c4-model-core-banking.drawio`<br>Context, Container, Component levels | ![C4 Model](sample-c4-model-core-banking.png) |
| **Domain-Driven Design**<br>`sample-domain-driven-design-core-banking.drawio`<br>DDD for ITMX core banking system | ![DDD](sample-domain-driven-design-core-banking.png) |
| **Sequence Diagram**<br>`sample-sequence-payment-flow.drawio`<br>Payment flow (Mobile to ITMX) | ![Sequence](sample-sequence-payment-flow.png) |

#### Database

| Diagram | Preview |
|---------|---------|
| **Entity-Relationship Diagram**<br>`sample-entity-property-agent-database.drawio`<br>Property agent ERD (PostgreSQL, MongoDB, Redis, Elasticsearch) | ![ERD](sample-entity-property-agent-database.png) |

## Usage

1. Copy `CLAUDE.md` to your project root or relevant subdirectory
2. Claude Code will automatically load the instructions when working in that directory
3. Ask Claude to create DrawIO diagrams - it will follow the guidelines
4. Reference sample diagrams for expected output format

## Arrow Color Standards

Protocol-based colors for consistent architecture diagrams:

| Protocol | Color | Hex |
|----------|-------|-----|
| HTTP/HTTPS/API | Gray | `#232F3E` |
| Database (SQL) | Purple | `#C925D1` |
| Cache (Redis) | Red | `#DC382D` |
| Message Queue | Pink | `#E7157B` |
| Storage | Green | `#7AA116` |
| Auth/Security | Dark Red | `#DD344C` |

## Diagram Types Supported

- Cloud Architecture (AWS, Azure, GCP)
- Kubernetes cluster architecture
- C4 Model (Context, Container, Component)
- Domain-Driven Design diagrams
- Sequence diagrams
- Network topology
- Data flow and ETL pipelines
- BPMN workflow diagrams
- Database ERD (multi-database: SQL, NoSQL, Cache, Search)
