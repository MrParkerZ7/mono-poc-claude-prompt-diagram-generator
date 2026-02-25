# CLAUDE.md Project-Specific Instructions Samples

A mono-repo containing sample `CLAUDE.md` files for different use cases with Claude Code.

## What is CLAUDE.md?

`CLAUDE.md` is a project-specific instruction file that Claude Code automatically loads when working in a directory. It allows you to:

- Define coding standards and conventions
- Provide domain-specific knowledge
- Set up shape/style references for diagram generation
- Configure project-specific workflows

## Projects

| Project | Description | Status |
|---------|-------------|--------|
| [claude-md-drawio](./claude-md-drawio/) | DrawIO architecture diagram generation | Active |
| [claude-md-excel](./claude-md-excel/) | Excel/spreadsheet generation | Planned |

## File Structure

```
mono-sample-claude-project-specific-instruction/
```

- [README.md](./README.md)
- [.gitignore](./.gitignore)

### [claude-md-drawio/](./claude-md-drawio/) — DrawIO diagram generation

| File | Description |
|------|-------------|
| [CLAUDE.md](./claude-md-drawio/CLAUDE.md) | Main instruction file (93KB) |
| [README.md](./claude-md-drawio/README.md) | Project documentation |

**Cloud Architecture Samples:**

| DrawIO | PNG Preview |
|--------|-------------|
| [sample-architecture-aws-ecs.drawio](./claude-md-drawio/sample-architecture-aws-ecs.drawio) | [PNG](./claude-md-drawio/sample-architecture-aws-ecs.png) |
| [sample-architecture-azure-webapp.drawio](./claude-md-drawio/sample-architecture-azure-webapp.drawio) | [PNG](./claude-md-drawio/sample-architecture-azure-webapp.png) |
| [sample-architecture-aws-portal-k8s-insurance.drawio](./claude-md-drawio/sample-architecture-aws-portal-k8s-insurance.drawio) | [PNG](./claude-md-drawio/sample-architecture-aws-portal-k8s-insurance.png) |
| [sample-architecture-insurance-cobroker-portal-k8s-onprem.drawio](./claude-md-drawio/sample-architecture-insurance-cobroker-portal-k8s-onprem.drawio) | [PNG](./claude-md-drawio/sample-architecture-insurance-cobroker-portal-k8s-onprem.png) |

**Software Architecture Samples:**

| DrawIO | PNG Preview |
|--------|-------------|
| [sample-c4-model-core-banking.drawio](./claude-md-drawio/sample-c4-model-core-banking.drawio) | [PNG](./claude-md-drawio/sample-c4-model-core-banking.png) |
| [sample-domain-driven-design-core-banking.drawio](./claude-md-drawio/sample-domain-driven-design-core-banking.drawio) | [PNG](./claude-md-drawio/sample-domain-driven-design-core-banking.png) |
| [sample-sequence-payment-flow.drawio](./claude-md-drawio/sample-sequence-payment-flow.drawio) | [PNG](./claude-md-drawio/sample-sequence-payment-flow.png) |

**Database Samples:**

| DrawIO | PNG Preview |
|--------|-------------|
| [sample-entity-property-agent-database.drawio](./claude-md-drawio/sample-entity-property-agent-database.drawio) | [PNG](./claude-md-drawio/sample-entity-property-agent-database.png) |

### [claude-md-excel/](./claude-md-excel/) — Excel generation (planned)

*Coming soon*

## claude-md-drawio

Comprehensive guidelines for generating professional DrawIO diagrams:

### Supported Diagram Types

- **Cloud Architecture**: AWS, Azure, GCP
- **Container Orchestration**: Kubernetes, Docker
- **Software Architecture**: C4 Model, DDD, Microservices
- **Database**: ERD with multi-database support (SQL, NoSQL, Cache, Search)
- **Workflows**: BPMN, Flowcharts, Sequence Diagrams
- **Network**: Infrastructure, Cisco, On-Premises

### Sample Diagrams

| Category | Samples |
|----------|---------|
| Cloud Architecture | AWS ECS, Azure WebApp, AWS+K8s Hybrid, On-Prem K8s |
| Software Architecture | C4 Model, Domain-Driven Design, Sequence Diagram |
| Database | Property Agent ERD (PostgreSQL, MongoDB, Redis, Elasticsearch) |

### Preview

![AWS ECS Architecture](./claude-md-drawio/sample-architecture-aws-ecs.png)

![C4 Model Core Banking](./claude-md-drawio/sample-c4-model-core-banking.png)

## Usage

1. Copy the relevant `CLAUDE.md` to your project root
2. Claude Code will automatically load it when working in that directory
3. Ask Claude to generate diagrams/files following the guidelines

## How CLAUDE.md Works

```
your-project/
├── CLAUDE.md          # Auto-loaded by Claude Code
├── src/
│   └── CLAUDE.md      # Also loaded when working in src/
└── ...
```

- Claude Code reads `CLAUDE.md` from project root automatically
- Subdirectory `CLAUDE.md` files are loaded when working in those directories
- All instructions are available in Claude's context without manual file reads

## License

MIT
