# DrawIO Diagram Spacing & Styling Rules

## Grid Configuration

- **Grid Size**: 10px (1 block = 10px)
- **All measurements should align to grid**

---

## Spacing Rules

### Group Container Spacing

| Spacing Type | Blocks | Pixels | Description |
|--------------|--------|--------|-------------|
| Between group containers | 2 | 20px | Gap between sibling groups |
| Group to sub-group border | 1 | 10px | Parent group edge to nested sub-group |
| Sub-group to sub-group | 2 | 20px | Gap between nested sub-groups |

### Shape Spacing

| Spacing Type | Blocks | Pixels | Description |
|--------------|--------|--------|-------------|
| Shape to group border | 2 | 20px | Shape edge to container border (left/right/bottom) |
| Shape to top of group | 4 | 40px | Space from top of group to first shape |
| Shape to shape (no text) | 2 | 20px | Gap between shapes without bottom text |
| Shape to shape (with text) | 4 | 40px | Gap between shapes when shape has bottom text |

---

## Shape Sizing

| Element | Blocks | Pixels |
|---------|--------|--------|
| Standard shape | 4x4 | 40x40 |
| Icon with label | 4x4 | 40x40 (+ 20px text below) |

---

## Layout Calculation Examples

### Single Column Container (e.g., Ingestion Layer)

```
Container width = 20 (left margin) + 40 (shape) + 20 (right margin) = 80px
```

### Two Column Container (e.g., Governance)

```
Container width = 20 (left) + 40 (shape1) + 20 (gap) + 40 (shape2) + 20 (right) = 140px
                  (actual: 160px for text labels)
```

### Nested Sub-groups (e.g., Storage Layer)

```
Parent container:
├── 10px (group to sub-group border)
├── Sub-group 1: 180px width
├── 20px (sub-group to sub-group gap)
├── Sub-group 2: 180px width
└── 10px (group to sub-group border)

Total width = 10 + 180 + 20 + 180 + 10 = 400px
```

### Vertical Shape Positions

```
y = 40   → Shape 1 (4 blocks from top of group)
y = 120  → Shape 2 (40 + 40 shape + 40 gap for text)
y = 200  → Shape 3 (120 + 40 shape + 40 gap)
y = 280  → Shape 4 (200 + 40 shape + 40 gap)

Without bottom text:
y = 40   → Shape 1
y = 100  → Shape 2 (40 + 40 shape + 20 gap)
y = 160  → Shape 3
```

---

## Container Height Calculation

```
With text labels (4 blocks gap between shapes):
Height = 40 (top margin)
       + (N shapes × 40)
       + ((N-1) × 40) for gaps between shapes (with text)
       + 20 (bottom margin)

Example (4 shapes with text):
= 40 + (4×40) + (3×40) + 20
= 40 + 160 + 120 + 20
= 340px

Without text labels (2 blocks gap between shapes):
Height = 40 (top margin)
       + (N shapes × 40)
       + ((N-1) × 20) for gaps between shapes
       + 20 (bottom margin)

Example (4 shapes without text):
= 40 + (4×40) + (3×20) + 20
= 40 + 160 + 60 + 20
= 280px
```

---

## Visual Reference

```
┌─────────────────────────────────────┐
│ Group Container                     │
│  ┌─────────────────────────────────┐│
│  │ 10px from parent               ││
│  │  ┌───────────┐  20px  ┌───────┐││
│  │  │ Sub-group │ ←────→ │ Sub-  │││
│  │  │ (label)   │        │ group │││
│  │  │ 40px top  │        │       │││
│  │  │  ┌────┐   │        │       │││
│  │  │  │    │   │        │       │││
│  │  │  │40x40   │        │       │││
│  │  │  │    │   │        │       │││
│  │  │  └────┘   │        │       │││
│  │  │  (text)   │        │       │││
│  │  │  40px gap │ ← with text     ││
│  │  │  (or 20px │ ← without text  ││
│  │  │  ┌────┐   │        │       │││
│  │  │  │next│   │        │       │││
│  │  │  └────┘   │        │       │││
│  │  │  20px     │        │       │││
│  │  │  margin   │        │       │││
│  │  └───────────┘        └───────┘││
│  │ 10px from parent               ││
│  └─────────────────────────────────┘│
│                                     │
│  ←──────── 20px gap ────────→       │
│                                     │
│  ┌─────────────────────────────────┐│
│  │ Next Group Container            ││
│  └─────────────────────────────────┘│
└─────────────────────────────────────┘
```

---

## Summary Table

| Rule | Value |
|------|-------|
| Grid block | 10px |
| Shape size | 40x40 (4x4 blocks) |
| Shape to group border | 20px (2 blocks) |
| Shape to top of group | 40px (4 blocks) |
| Shape to shape (no text) | 20px (2 blocks) |
| Shape to shape (with text) | 40px (4 blocks) |
| Group to group gap | 20px (2 blocks) |
| Group to sub-group | 10px (1 block) |
| Sub-group to sub-group | 20px (2 blocks) |
