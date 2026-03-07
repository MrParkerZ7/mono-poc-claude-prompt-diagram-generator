# DrawIO Architecture Diagram Generation with Claude

A collection of prompt templates, style guides, and sample diagrams for generating professional DrawIO architecture diagrams using Claude Code.

## Overview

This repository provides everything needed to generate consistent, professional-quality architecture diagrams:

- **Prompt Templates** - Ready-to-use prompts for diagram generation and project analysis
- **Style Guides** - Formatting standards for spacing, sizing, and visual consistency
- **Sample Diagrams** - Real-world examples across different diagram types
- **Project Analysis Examples** - Complete analysis-to-diagram workflows

## Repository Structure

```
mono-sample-claude-project-specific-instruction/
├── CLAUDE.md                      # Project instructions for Claude Code
├── README.md                      # This file
│
├── _PROMPT/                       # Prompt Templates
│   ├── PROMPT-DIAGRAM-FORMAT.md   # DrawIO shape/style reference (158KB)
│   ├── PROMPT-PROJECT-ANALYSIS.md # Project analysis templates
│   ├── PROMPT_TEMPLATE.md         # Quick-start prompt
│   └── PROMPT_TEMPLATE_DOCS.md    # Documentation prompt
│
├── format-group-icon-space-size-standard/  # Formatting Style Guides
│   ├── PROMPT_STYLE.md                     # Universal spacing, sizing, arrow rules
│   ├── PROMPT_OBJECT_FORMAT.md             # Object format reference
│   └── sample-*.drawio                     # Format-compliant samples
│
├── poc-architecture/              # Cloud Architecture Samples
├── poc-c4-model/                  # C4 Model Samples
├── poc-datalake/                  # Data Lake/Warehouse Samples
├── poc-domain-driven-design/      # DDD Bounded Context Samples
├── poc-entity-erd/                # Database ERD Samples
├── poc-sequence/                  # Sequence Diagram Samples
│
└── poc-project-*/                 # Full Project Analysis Examples
    ├── analysis/                  # Analysis documents
    └── diagrams/                  # Generated diagrams
```

## Diagram Types

| Type | Folder | Description |
|------|--------|-------------|
| Cloud Architecture | `poc-architecture/` | AWS, Azure, GCP, Kubernetes, Hybrid |
| C4 Model | `poc-c4-model/` | Context, Container, Component diagrams |
| Data Lake/Warehouse | `poc-datalake/` | ETL pipelines, data flow, analytics |
| Domain-Driven Design | `poc-domain-driven-design/` | Bounded contexts, aggregates |
| Entity ERD | `poc-entity-erd/` | Database schemas, relationships |
| Sequence Diagrams | `poc-sequence/` | API flows, interactions |

## Quick Start

### Option 1: Use Prompt Templates

1. Copy the prompt from `_PROMPT/PROMPT_TEMPLATE.md`
2. Replace `[YOUR_PROJECT_PATH]` with your project path
3. Paste into Claude Code

### Option 2: Copy CLAUDE.md to Your Project

1. Copy `_PROMPT/PROMPT-DIAGRAM-FORMAT.md` to your project as `CLAUDE.md`
2. Claude Code will auto-load the instructions
3. Ask Claude to analyze and create diagrams

## Key Features

### Comprehensive Shape Library
- AWS, Azure, GCP service icons
- Kubernetes resources
- Database types (SQL, NoSQL, Cache)
- C4 Model elements
- Network & infrastructure components

### Consistent Styling
- Protocol-based arrow colors (HTTP, Database, Queue, etc.)
- Arrow Legend and Component Legend (MANDATORY)
- Grid-aligned positioning (10px grid)
- Shadow and text shadow standards

### Format Standards
- Icon size: 60x60px minimum
- Container positions: multiples of 10px
- Stroke width: 2px standard
- Required attributes: `shadow=1;textShadow=1`
- All elements must have connections (no orphans)

### Universal Diagram Support
`PROMPT_STYLE.md` supports all diagram types:
- Cloud Architecture (AWS, Azure, GCP, Kubernetes)
- C4 Model (Context, Container, Component)
- Sequence Diagrams
- ERD (Entity-Relationship)
- Data Flow / ETL
- Flowchart / BPMN
- State Machine

## Sample Projects

| Project | Tech Stack | Diagrams |
|---------|------------|----------|
| AWS Terraform Lambda | Python, Terraform, AWS | Infrastructure, C4, Sequence |
| Lerna Node Monorepo | TypeScript, Node.js | Architecture, Dependencies |
| Kotlin Spring MongoDB | Kotlin, Spring, MongoDB | API, Domain Model, ERD |

## Color Scheme Reference

| Category | Fill | Stroke |
|----------|------|--------|
| Compute (AWS Lambda/ECS) | `#FFF4E6` | `#ED7100` |
| Database | `#FAE6FC` | `#C925D1` |
| Storage | `#E9F3E6` | `#7AA116` |
| Messaging | `#FCE8F3` | `#E7157B` |
| Security | `#FCE8EB` | `#DD344C` |
| Networking | `#E6F2FA` | `#8C4FFF` |

## License

MIT
