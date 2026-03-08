# DrawIO Layout Pattern Guide

## Overview

Layout patterns extracted from well-structured architecture diagrams. These patterns complement `PROMPT_OBJECT_FORMAT.md` (minimum rules) with **recommended values** that produce clean, readable diagrams.

Reference diagrams:
- `poc-architecture/sample-architecture-aws-ecs.drawio`
- `poc-architecture/sample-architecture-aws-portal-k8s-insurance.drawio`

---

## 1. Arrow Style (CRITICAL)

**Use straight-line auto-routed arrows — NOT orthogonal, NOT entity relation.**

```xml
<mxCell id="conn-1" style="edgeStyle=none;rounded=0;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=classic;endFill=1;shadow=1;textShadow=1;"
  parent="1" source="source-id" target="target-id" edge="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

| Property | Value | Why |
|----------|-------|-----|
| `edgeStyle` | `none` | Straight lines, auto-routed from center-to-center |
| `rounded` | `0` | Clean line segments |
| Waypoints | None | Let DrawIO auto-calculate optimal path |
| exitX/exitY | None | Auto-detected from icon boundaries |
| entryX/entryY | None | Auto-detected from icon boundaries |

> **Why not orthogonalEdgeStyle?** Orthogonal creates multiple right-angle bends that clutter dense diagrams. Straight lines are cleaner and easier to follow.
>
> **Why not entityRelationEdgeStyle?** ER style is for ERD table-to-table connections only. It creates messy L-shaped bends in architecture diagrams with nested groups.

---

## 2. Nesting Margins (CONSISTENT)

Every parent → child container follows the same spacing:

| Margin | Value | Description |
|--------|-------|-------------|
| Left margin | **20px** | From parent left edge to child left edge |
| Top margin | **40px** | From parent top edge to child top edge (includes title space) |
| Right padding | **20px** | From content right edge to parent right edge |
| Bottom padding | **20px** | From content bottom edge to parent bottom edge |

```
┌─ Parent Group (title) ──────────────┐
│ 40px top                            │
│   ┌─ Child Group ──────────┐  20px  │
│   │                        │  right │
│   │                        │        │
│   └────────────────────────┘        │
│ 20px bottom                         │
└─────────────────────────────────────┘
  20px left
```

**Example nesting (absolute positions):**
```
AWS Cloud:    x=160, y=80
  Region:     x=20,  y=40  (relative to Cloud)
    VPC:      x=20,  y=40  (relative to Region)
      Subnet: x=20,  y=40  (relative to VPC)
```

---

## 3. Icon Spacing

### Within Groups (Horizontal Row Layout)

| Rule | Value |
|------|-------|
| First icon position | `x=30, y=30` from group edge |
| Center-to-center horizontal | **120px** |
| Icon size | 60×60px |
| Edge-to-edge gap | 60px (120 - 60) |

```
┌─ Group ──────────────────────────────────────┐
│  30px  ┌──┐  60px  ┌──┐  60px  ┌──┐  30px   │
│        │60│        │60│        │60│          │
│        └──┘        └──┘        └──┘          │
└──────────────────────────────────────────────┘
         x=30       x=150       x=270
```

### Within Groups (Vertical Column Layout)

| Rule | Value |
|------|-------|
| First icon position | `x=60, y=40` (centered in 180px group) |
| Center-to-center vertical | **90px** |
| Edge-to-edge gap | 30px (90 - 60) |

```
┌─ Subnet (180px wide) ─┐
│       ┌──┐  y=40       │
│       │60│              │
│       └──┘              │
│       30px gap          │
│       ┌──┐  y=130      │
│       │60│              │
│       └──┘              │
│       30px gap          │
│       ┌──┐  y=220      │
│       │60│              │
│       └──┘              │
└────────────────────────┘
        x=60
```

### Multi-Column Grid (e.g., Kubernetes Cluster)

| Rule | Value |
|------|-------|
| Horizontal center-to-center | **80px** |
| Vertical center-to-center | **110px** |

---

## 4. Group Sizing Formulas

### Width

| Layout | Formula | Example (3 icons) |
|--------|---------|-------------------|
| Single icon column | **180px** fixed | 180px |
| Horizontal row | `(N × 120)` px | 3 × 120 = 360px |
| Sub-groups side-by-side | Sum of sub-widths + 20px gaps | 360 + 20 + 480 = 860px |

### Height

| Layout | Formula | Example |
|--------|---------|---------|
| Single row of icons | **130px** | 40(title) + 60(icon) + 30(label+margin) |
| Containing sub-groups | `40 + sub_height + 20` | 40 + 130 + 20 = 190px |
| Vertical stack (N icons) | `40 + N×90 - 30 + 20` | 3 icons: 40 + 270 - 30 + 20 = 300px |

---

## 5. Gap Between Sibling Groups

| Direction | Gap | Description |
|-----------|-----|-------------|
| Horizontal (side-by-side) | **20px** | Between adjacent columns/groups |
| Vertical (stacked rows) | **20px** | Between adjacent row groups |

---

## 6. Row Width Balance (CRITICAL for Complex Diagrams)

When a container has multiple sub-groups or many icons, **don't force everything into one row**. Use the wrap rule to keep the layout balanced.

### Wrap Rule

```
MAX_ROW_WIDTH = 1200px (threshold)

1. Calculate total row width = sum(sub-group widths) + gaps
2. If total > MAX_ROW_WIDTH → wrap into multiple rows
3. Max 2 sub-groups per row
4. Pair sub-groups to minimize max row width
```

### Width Formula

```
sub-group width     = N_icons × 120 + 60
row width           = sub_width_A + 20(gap) + sub_width_B
parent content width = max(row_widths across all rows)
parent total width  = content_width + 40(margins)
```

### Height Formula (Multi-Row)

```
Single row of sub-groups:  40(title) + 130(sub) + 20(bottom) = 190px
Two rows of sub-groups:    40 + 130 + 20(gap) + 130 + 20 = 340px
Three rows:                40 + 130 + 20 + 130 + 20 + 130 + 20 = 490px
```

### Example: Banking Core Services (4 sub-groups)

**Before (single row — too wide):**
```
┌─ Core Banking (2260px wide!) ────────────────────────────────────────────┐
│ ┌─Account(540)─┐ ┌─Transaction(660)─┐ ┌─Lending(540)─┐ ┌─Card(420)─┐  │
│ └──────────────┘ └──────────────────┘ └──────────────┘ └───────────┘   │
└──────────────────────────────────────────────────────────────────────────┘
```

**After (wrapped 2×2 — balanced at 1220px):**
```
┌─ Core Banking (1260px wide, 340px tall) ──────────────┐
│ ┌─Account(540)─┐ ┌─Transaction(660)─┐                │
│ └──────────────┘ └──────────────────┘                 │
│ ┌─Lending(540)─┐ ┌─Card(420)────────┐                │
│ └──────────────┘ └──────────────────┘                 │
└───────────────────────────────────────────────────────┘
```

### Pairing Strategy

When wrapping N sub-groups into rows of 2, pair to **minimize the widest row**:

```
1. Sort sub-groups by width descending
2. Pair largest with smallest:
   - [660, 540, 540, 420] → (660+420=1080), (540+540=1080) ← optimal
   - Or keep semantic pairs: (540+660=1200), (540+420=960) ← acceptable
```

### VPC Width Rule

```
VPC_content_width = max(all subnet content widths)
VPC_width         = VPC_content_width + 40
All subnets       = VPC_content_width (consistent width)
```

This means narrower subnets (e.g., Public with 3 icons = 420px content) will have empty space on the right — this is expected and acceptable since it maintains consistent subnet boundaries within the VPC.

---

## 7. Layout Strategy by Diagram Complexity

### Simple (≤15 icons): Column Layout
Subnets as **vertical columns**, side by side. Icons stack vertically within each column.

```
┌─ VPC ─────────────────────────────────┐
│ ┌─Public─┐ ┌─Private─┐ ┌─Data────┐   │
│ │  IGW   │ │ API Svc │ │  RDS    │   │
│ │  ALB   │ │ Fargate │ │  S3     │   │
│ │  NAT   │ │ Worker  │ │  Cache  │   │
│ └────────┘ └─────────┘ └─────────┘   │
└───────────────────────────────────────┘
```

### Complex (>15 icons): Row Layout with Wrap Rule
Subnets as **horizontal rows**, stacked vertically. Apply the **Wrap Rule** from Section 6 to keep rows balanced.

```
┌─ VPC ────────────────────────────────────────┐
│ ┌─ Public Subnet ──────────────────────────┐ │
│ │  ALB    API GW    NAT                    │ │
│ └──────────────────────────────────────────┘ │
│ ┌─ Core Subnet (wrapped 2×2) ──────────────┐ │
│ │  ┌─Group A───┐ ┌─Group B────────┐        │ │
│ │  │ Svc1 Svc2 │ │ Svc3 Svc4 Svc5 │        │ │
│ │  └───────────┘ └────────────────┘         │ │
│ │  ┌─Group C───┐ ┌─Group D──┐               │ │
│ │  │ Svc6 Svc7 │ │ Svc8 Svc9│               │ │
│ │  └───────────┘ └──────────┘               │ │
│ └──────────────────────────────────────────┘ │
│ ┌─ Data Subnet (wrapped) ─────────────────┐ │
│ │  ┌─DB Group──────────────────┐           │ │
│ │  │ RDS  DynamoDB  Cache      │           │ │
│ │  └──────────────────────────┘            │ │
│ │  ┌─Storage Group─────┐                   │ │
│ │  │ S3   Glacier  EFS │                   │ │
│ │  └───────────────────┘                   │ │
│ └──────────────────────────────────────────┘ │
└──────────────────────────────────────────────┘
```

---

## 7. Stroke Width Hierarchy (Optional)

Visual hierarchy through border weight:

| Nesting Level | strokeWidth | Example |
|---------------|-------------|---------|
| Top-level container | **4px** | AWS Cloud, Data Center |
| Region / Network | **3px** | Region, Corporate Network |
| Subnet / Zone | **2px** | Public Subnet, Private Subnet |

> Note: `PROMPT_OBJECT_FORMAT.md` standardizes all at 2px. Use hierarchy only when visual distinction between nesting levels is needed.

---

## 8. External Elements Positioning

### Users / Actors (Outside Cloud)
- Position to the **left** of the cloud boundary
- Vertically aligned with the services they connect to
- `x = 40px` (standard left position)

### Security / External Services
- Position to the **right** of VPC, inside Region
- Stacked vertically in a column

```
┌─ Region ───────────────────────────────────┐
│ ┌─ VPC ──────────────┐ ┌─ Security ─────┐ │
│ │                     │ │  Cognito       │ │
│ │  (main services)    │ │  KMS           │ │
│ │                     │ │  IAM           │ │
│ └─────────────────────┘ └────────────────┘ │
│                          ┌─ External ─────┐ │
│                          │  SWIFT         │ │
│                          │  Payment Net   │ │
│                          └────────────────┘ │
└────────────────────────────────────────────┘
```

---

## 9. Legend Positioning

| Rule | Value |
|------|-------|
| Position | **Below** main diagram content |
| Gap from content | **40px** minimum |
| Arrow Legend width | 340-500px |
| Legend row height | **25px** per entry |
| Component Legend | Side-by-side with Arrow Legend |

---

## 11. Validation Checklist

| Check | Rule |
|-------|------|
| ☐ | All arrows use `edgeStyle=none;rounded=0` |
| ☐ | No manual waypoints or exitX/exitY |
| ☐ | Nesting margins: 20px left, 40px top |
| ☐ | Icon spacing: 120px horizontal c-to-c |
| ☐ | Icon spacing: 90px vertical c-to-c |
| ☐ | Groups sized by formula (not arbitrary) |
| ☐ | Sibling group gap: 20px |
| ☐ | **No row wider than 1200px** (wrap if exceeded) |
| ☐ | **Sub-groups wrapped 2 per row max** |
| ☐ | Legend 40px below content |
| ☐ | All positions/sizes multiples of 10px |

---

## Quick Reference

```
Nesting margin:      20px left, 40px top
Icon first pos:      x=30, y=30 (in group)
Horizontal c-to-c:   120px
Vertical c-to-c:     90px
Group gap:           20px
Single column width:  180px
Row height:          130px
Max row width:       1200px (wrap if exceeded)
Multi-row height:    340px (2 rows), 490px (3 rows)
Arrow style:         edgeStyle=none;rounded=0
```
