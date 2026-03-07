# CLAUDE.md - Project Instructions

## Project Overview

This repository is a **prompt engineering toolkit** for generating professional DrawIO architecture diagrams with Claude Code. It contains:

- Comprehensive diagram format standards (shapes, styles, colors)
- Reusable prompt templates for project analysis
- Style guides for consistent formatting
- Sample diagrams as references

## Repository Structure

```
mono-sample-claude-project-specific-instruction/
├── CLAUDE.md                      # This file
├── README.md                      # Project documentation
│
├── _PROMPT/                       # Core Prompt Templates
│   ├── PROMPT-DIAGRAM-FORMAT.md   # Main DrawIO standards (shapes, colors, arrows)
│   ├── PROMPT-PROJECT-ANALYSIS.md # Project analysis workflow
│   ├── PROMPT_TEMPLATE.md         # Quick-start template
│   └── PROMPT_TEMPLATE_DOCS.md    # Documentation generation
│
├── format-architect/              # Formatting Style Guides
│   ├── PROMPT_STYLE.md            # Spacing, sizing, grid rules
│   └── sample-*.drawio            # Format-compliant examples
│
├── poc-{diagram-type}/            # Diagram Type Samples
│   ├── README.md                  # Type-specific instructions
│   └── sample-*.drawio            # Sample diagrams
│
└── poc-project-{name}/            # Full Project Examples
    ├── analysis/                  # Analysis documents
    └── diagrams/                  # Generated diagrams
```

## Key Files

| File | Purpose |
|------|---------|
| `_PROMPT/PROMPT-DIAGRAM-FORMAT.md` | Main DrawIO standards - shapes, styles, colors, arrows |
| `_PROMPT/PROMPT-PROJECT-ANALYSIS.md` | Analysis workflow and output templates |
| `format-architect/PROMPT_STYLE.md` | Spacing and sizing rules |

## Working with This Repository

### Adding New Diagram Samples

1. **For diagram type samples:** Add to `poc-{type}/` folder
2. **For full project examples:** Create `poc-project-{name}/` with `analysis/` and `diagrams/` subfolders

### Diagram Naming Convention

| Type | Pattern | Example |
|------|---------|---------|
| Architecture | `sample-architecture-{name}.drawio` | `sample-architecture-aws-ecs.drawio` |
| C4 Model | `sample-c4-model-{name}.drawio` | `sample-c4-model-core-banking.drawio` |
| ERD | `sample-entity-{name}.drawio` | `sample-entity-property-agent.drawio` |
| Sequence | `sample-sequence-{name}.drawio` | `sample-sequence-payment-flow.drawio` |

### Format Standards (MANDATORY)

All diagrams must follow these rules from `format-architect/PROMPT_STYLE.md`:

| Rule | Requirement |
|------|-------------|
| Grid | 10px (all positions/sizes must end with 0) |
| Icon size | 60x60px minimum |
| Stroke width | 2px standard |
| Shadows | `shadow=1;textShadow=1` on all elements |
| Legend | Required for all meaningful arrow styles |

### Color Scheme

| Category | Fill | Stroke |
|----------|------|--------|
| Compute | `#FFF4E6` | `#ED7100` |
| Database | `#FAE6FC` | `#C925D1` |
| Storage | `#E9F3E6` | `#7AA116` |
| Messaging | `#FCE8F3` | `#E7157B` |
| Security | `#FCE8EB` | `#DD344C` |

## Git Commit Guidelines

- Start with action verb: Add, Update, Fix, Remove
- Reference diagram type in commits
- Co-author with Claude when applicable:
  ```
  Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
  ```

## Updating Prompt Templates

When updating `_PROMPT/` files:
1. Update Table of Contents if adding sections
2. Maintain consistent heading structure
3. Include XML examples for new diagram types
4. Test with sample diagram generation
