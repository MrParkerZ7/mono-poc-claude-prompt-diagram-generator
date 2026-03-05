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
| Shape to group border | 2 | 20px | Shape edge to container border |
| Shape with bottom text | +2 | +20px | Additional space below for label |
| Shape to shape (vertical) | - | 70px | 40 (shape) + 20 (text) + 10 (gap) |

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
y = 20   → Shape 1 (top margin: 2 blocks)
y = 90   → Shape 2 (20 + 40 shape + 20 text + 10 gap)
y = 160  → Shape 3
y = 230  → Shape 4
```

---

## Container Height Calculation

```
Height = 20 (top margin)
       + (N shapes × 40)
       + ((N shapes) × 20) for text labels
       + ((N-1) × 10) for gaps between shapes
       + 20 (bottom margin)

Example (4 shapes):
= 20 + (4×40) + (4×20) + (3×10) + 20
= 20 + 160 + 80 + 30 + 20
= 310px
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
│  │  │           │        │ group │││
│  │  │  ┌────┐   │        │       │││
│  │  │  │    │   │        │       │││
│  │  │  │40x40   │        │       │││
│  │  │  │    │   │        │       │││
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
| Group to group gap | 20px (2 blocks) |
| Group to sub-group | 10px (1 block) |
| Sub-group to sub-group | 20px (2 blocks) |
| Bottom text space | 20px (2 blocks) |
