# Format Architect - Spacing Style Guide

## Overview

This document defines **minimum spacing rules** for infrastructure architecture diagrams. Objects can have more space between them as needed for visual clarity.

## Grid System

- Grid size: `10px` (1 block)
- All spacing values are **minimums**

### Grid Snap Rule (MANDATORY)

**All group container positions and sizes MUST end with `0` (multiples of 10px)**

| Property | Rule | Valid Examples | Invalid Examples |
|----------|------|----------------|------------------|
| Position X | Must end with 0 | `10`, `20`, `100`, `250` | `15`, `23`, `108`, `255` |
| Position Y | Must end with 0 | `10`, `20`, `100`, `250` | `15`, `23`, `108`, `255` |
| Width | Must end with 0 | `60`, `100`, `200`, `450` | `65`, `98`, `215`, `448` |
| Height | Must end with 0 | `60`, `100`, `200`, `450` | `65`, `98`, `215`, `448` |

**Why:** Ensures all containers snap to the 10px grid for consistent alignment and clean visual appearance.

## Minimum Spacing Rules

### Containers

| Rule | Minimum Value | Description |
|------|---------------|-------------|
| Margin from parent | `10px` (1 block) | Minimum distance from parent container edge |
| Title padding | `+20px` (2 blocks) | Additional top space for containers with titles |
| Content start Y | `30px` minimum | For titled containers: 10px margin + 20px title |

### Shapes/Icons

| Rule | Minimum Value | Description |
|------|---------------|-------------|
| Margin from edges | `10px` (1 block) | Minimum distance from container edge |
| Bottom label space | `+20px` (2 blocks) | Additional space for shapes with bottom text labels |
| Horizontal gap | `30px` minimum | Minimum space between adjacent icons (edge-to-edge) |

### Icon Size Standard

**Rule: The shorter side must be at least 6 blocks (60px)**

| Aspect Ratio | Size (blocks) | Size (px) | Example |
|--------------|---------------|-----------|---------|
| 1:1 (square) | 6 x 6 | 60 x 60 | Standard service icons |
| 1:2 | 6 x 12 | 60 x 120 | Tall icons (e.g., person, server rack) |
| 2:1 | 12 x 6 | 120 x 60 | Wide icons (e.g., load balancer banner) |
| 2:3 | 6 x 9 | 60 x 90 | Slightly tall icons |

**Calculation:** `shorter_side = 6 blocks (60px)`, then scale the longer side proportionally.

### Container Height Guidelines

| Content | Minimum Height | Calculation |
|---------|----------------|-------------|
| Single row (6x6 icons) | `120px` | 30 (title) + 60 (icon) + 20 (label) + 10 (margin) |
| Two rows (6x6 icons) | `210px` | 30 + (60+20)×2 + 10 gap + 10 margin |

## Key Principles

1. **These are minimums** - Use more space when it improves readability
2. **Visual balance** - Adjust spacing for visual harmony, not strict grid alignment
3. **Content-driven** - Let the content determine actual spacing needs
4. **Consistency within groups** - Similar elements should have consistent spacing
