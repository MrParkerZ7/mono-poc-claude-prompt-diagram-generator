# DrawIO Arrow Cable Management Guide

## Overview

Arrow routing standard inspired by **electric wire cable management** in high-rise buildings and industrial infrastructure. Instead of letting arrows auto-route freely (causing overlaps, diagonal crossings, and visual clutter), this guide organizes all arrows into **cable channels** — shared corridors with defined join/split points.

This guide works with `edgeStyle=orthogonalEdgeStyle` (90-degree routing only) and provides two levels of routing control:

**Level 1 — Entry/Exit Point Routing** (preferred, simpler):
- **16-point connection grid** — explicit exit/entry points on element boundaries
- **Fan-out/fan-in separation** — spread arrows across boundary using quarter-points
- **Direction-based rules** — perpendicular face pairs for diagonal connections
- **Cross-container offsets** — offset points to avoid overlap at container boundaries

**Level 2 — Waypoint Cable Management** (for complex diagrams):
- **Cable trays** — shared horizontal/vertical corridors
- **Join/split points** — where arrows enter/leave a trunk line
- **Zero-overlap guarantee** — no two wire segments ever share the same line
- **Jump arcs** — where wires cross perpendicularly, show an arc (electrical schematic standard)
- **Vertical risers** — dedicated vertical corridors passing through intermediate layers

Complements:
- `PROMPT_OBJECT_FORMAT.md` — element sizing, spacing, arrow color rules
- `PROMPT_LAYOUT_PATTERN.md` — layout patterns, nesting margins

Reference diagrams:
- `format-object-layout-standard/sample-architecture-banking.drawio`
- `format-object-layout-standard/sample-traning-arrow-pattern.drawio` (training dataset with proposition/pattern pairs)

---

## When to Use

| Scenario | Recommended Style |
|----------|-------------------|
| Simple diagram (≤15 icons, ≤10 arrows) | `edgeStyle=none` (straight lines) — see `PROMPT_LAYOUT_PATTERN.md` |
| Complex diagram (>15 icons, >15 arrows) | `edgeStyle=orthogonalEdgeStyle` + cable management (this guide) |
| Diagrams with many crossing arrows | Cable management with `jumpStyle=arc` |
| Diagrams with arrows spanning multiple layers | Cable management with vertical risers |

---

## Edge Style Settings

### Base Style (MANDATORY for cable management)

```xml
<mxCell id="conn-1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=classic;endFill=1;shadow=1;jumpStyle=arc;jumpSize=10;"
  parent="1" source="source-id" target="target-id" edge="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="..." y="..."/>
    </Array>
  </mxGeometry>
</mxCell>
```

| Property | Value | Purpose |
|----------|-------|---------|
| `edgeStyle` | `orthogonalEdgeStyle` | 90-degree routing only (no diagonals) |
| `rounded` | `0` | Sharp 90-degree bends (like real conduit) |
| `jumpStyle` | `arc` | Arc jump where wires cross |
| `jumpSize` | `10` | 10px arc radius at crossings |
| Waypoints | `<Array as="points">` | Explicit intermediate routing points |

---

## CRITICAL: Entry/Exit Point Routing

> **Before using waypoints, first control arrow paths with `exitX`, `exitY`, `entryX`, `entryY` on edges.**

Entry/exit points define WHERE an arrow leaves its source and WHERE it enters its target, using the element's **16-point connection grid**. This is the primary technique for clean arrow routing — simpler and more maintainable than waypoints.

### The 16-Point Connection Grid

Every node with the standard `points=[[...]]` style has 16 connection points:

```
        (0,0)  (0.25,0) (0.5,0) (0.75,0)  (1,0)
          ┌────────┬────────┬────────┬────────┐
 (0,0.25) │        │        │        │        │ (1,0.25)
          ├────────┤        │        ├────────┤
  (0,0.5) │        │   ICON CENTER   │        │ (1,0.5)
          ├────────┤        │        ├────────┤
 (0,0.75) │        │        │        │        │ (1,0.75)
          └────────┴────────┴────────┴────────┘
        (0,1)  (0.25,1) (0.5,1) (0.75,1)  (1,1)
```

### Edge Style with Entry/Exit Points

```xml
<mxCell id="e1" style="edgeStyle=orthogonalEdgeStyle;html=1;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;"
  edge="1" parent="..." source="..." target="...">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

| Property | Description |
|----------|-------------|
| `exitX`, `exitY` | Point on source boundary where arrow leaves (0-1 range) |
| `entryX`, `entryY` | Point on target boundary where arrow arrives (0-1 range) |
| `exitDx`, `exitDy` | Always `0` (no pixel offset) |
| `entryDx`, `entryDy` | Always `0` (no pixel offset) |

### Direction-Based Entry/Exit Rules

| Flow Direction | Exit Point | Entry Point | Notes |
|----------------|------------|-------------|-------|
| Left → Right (direct) | `exitX=1;exitY=0.5` | `entryX=0;entryY=0.5` | Default, often auto-routes correctly |
| Right → Left | `exitX=0;exitY=0.5` | `entryX=1;entryY=0.5` | Reverse of above |
| Top → Bottom | `exitX=0.5;exitY=1` | `entryX=0.5;entryY=0` | Vertical downward flow |
| Bottom → Top | `exitX=0.5;exitY=0` | `entryX=0.5;entryY=1` | Vertical upward flow |
| Diagonal ↘ (down-right) | `exitX=0.5;exitY=1` | `entryX=0;entryY=0.5` | Exit bottom, enter left face |
| Diagonal ↙ (down-left) | `exitX=0.5;exitY=1` | `entryX=1;entryY=0.5` | Exit bottom, enter right face |
| Diagonal ↗ (up-right) | `exitX=0.5;exitY=0` | `entryX=0;entryY=0.5` | Exit top, enter left face |
| Diagonal ↖ (up-left) | `exitX=0.5;exitY=0` | `entryX=1;entryY=0.5` | Exit top, enter right face |

**Key principle:** For diagonal connections, use **perpendicular face pairs** — exit from the face closest to the target's general direction, enter from the face facing back toward the source.

### Fan-Out Rules (1 Source → Multiple Targets)

When multiple arrows leave the same source, **spread exit points** across the source boundary to prevent overlap:

```
[Source] ──→ Target A (top)       exitX=1;exitY=0.25  (right face, upper quarter)
         ──→ Target B (middle)    (no exit — auto-routes center)
         ──→ Target C (bottom)    exitX=1;exitY=0.75  (right face, lower quarter)
```

| # of Outgoing | Exit Points Used |
|----------------|-----------------|
| 2 arrows right | `(1, 0.25)` and `(1, 0.75)` |
| 3 arrows right | `(1, 0.25)`, `(1, 0.5)` auto, `(1, 0.75)` |
| 2 arrows down | `(0.25, 1)` and `(0.75, 1)` |
| 3 arrows down | `(0.25, 1)`, `(0.5, 1)` auto, `(0.75, 1)` |
| Mixed directions | Use different faces: right `(1, 0.5)`, bottom `(0.5, 1)`, top `(0.5, 0)` |

**The middle/direct-path target** can use auto-routing (no explicit exit/entry). Only the offset targets need explicit points.

### Fan-In Rules (Multiple Sources → 1 Target)

When multiple arrows enter the same target, **spread entry points** across the target boundary:

```
Source A (left)  ──→ [Target]     entryX=0;entryY=0.25  (left face, upper quarter)
Source B (left)  ──→              entryX=0;entryY=0.75  (left face, lower quarter)
Source C (above) ──→              entryX=0.5;entryY=0   (top face, center)
```

| Scenario | Entry Point Strategy |
|----------|---------------------|
| 2 arrows from same side | Use `0.25` and `0.75` on that face |
| 3 arrows from same side | Use `0.25`, `0.5`, `0.75` on that face |
| Arrows from different sides | Each uses center of its own face (`0.5`) |
| Cross-lane + direct | Direct at `(0, 0.5)`, cross-lane at `(0, 0.25)` |

### Cross-Container Routing

When arrows cross container (group) boundaries, use **offset entry/exit points** to avoid overlapping with intra-container arrows:

```xml
<!-- Arrow crossing from VPC-A into VPC-B (cross-container) -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;html=1;entryX=0;entryY=0.75;" .../>
                                                          ↑
                                        offset from 0.5 to 0.75 to avoid overlap
                                        with direct arrow entering at 0.5
```

| Cross-Container Scenario | Entry/Exit Strategy |
|--------------------------|---------------------|
| Sibling containers (same level) | Entry at `0.25` or `0.75` to offset from direct arrows |
| Parent → child container | Exit parent bottom `(0.5, 1)`, enter child top `(0.5, 0)` |
| Cross-region (dashed red) | Same rules, but use `dashed=1;strokeColor=#FF0000` on edge |
| Through Transit Gateway/hub | Hub exit face = toward target account's direction |

### Cycle Routing (A → B → C → D → A)

For square/diamond cycle layouts, use perpendicular face pairs to keep arrows clean:

```
        [A top]
       ↗       ↘
   [D left]   [B right]        Clockwise: A→B→C→D→A
       ↖       ↙
        [C bottom]
```

| Segment | Exit | Entry |
|---------|------|-------|
| A → B (top → right) | auto | auto |
| B → C (right → bottom) | `exitX=0.5;exitY=1` | `entryX=1;entryY=0.5` |
| C → D (bottom → left) | auto | auto |
| D → A (left → top) | `exitX=0.5;exitY=0` | `entryX=0;entryY=0.5` |

**Principle:** Alternate between auto-routed segments and explicitly routed segments. The perpendicular-face exits/entries prevent the orthogonal router from creating overlapping L-shapes.

### When NOT to Use Explicit Entry/Exit

Auto-routing is sufficient (no `exitX`/`entryX` needed) for:

| Scenario | Why Auto-Routing Works |
|----------|----------------------|
| Direct horizontal (same row) | Router picks left/right faces automatically |
| Direct vertical (same column) | Router picks top/bottom faces automatically |
| Simple bidirectional (2 aligned nodes) | Router separates the two arrows automatically |
| Middle target in symmetric fan-out | Direct path needs no override |
| Simple fan-in from one side | Router distributes entry points automatically |

### Connection Points Usage Frequency

From training data analysis, the most effective connection points:

| Point | Coordinates | Primary Use Case |
|-------|-------------|-----------------|
| Right-center | `(1, 0.5)` | Default L→R exit; diagonal entry from right |
| Left-center | `(0, 0.5)` | Default L→R entry; most common explicit entry |
| Top-center | `(0.5, 0)` | Upward exit; downward entry |
| Bottom-center | `(0.5, 1)` | Downward exit in cycles/diagonals |
| Right-upper | `(1, 0.25)` | Fan-out exit for upward-bound target |
| Right-lower | `(1, 0.75)` | Fan-out exit for downward-bound target |
| Left-upper | `(0, 0.25)` | Fan-in entry, offset from center; cross-lane entry |
| Left-lower | `(0, 0.75)` | Cross-container entry, offset from center |
| Top-left | `(0.25, 0)` | Entry from above, offset to avoid overlap |
| Top-right | `(0.75, 0)` | Entry from above, opposite offset |

**Rarely used:** Corner points `(0,0)`, `(1,0)`, `(0,1)`, `(1,1)` — these create awkward 45° approach angles with orthogonal routing.

### Entry/Exit + Waypoints Combined

For complex routing, combine entry/exit points with waypoints. Entry/exit controls WHERE the arrow connects; waypoints control the PATH in between:

```xml
<!-- Fan-out with cable channel: exit upper-right, route through vertical channel -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;html=1;exitX=1;exitY=0.25;entryX=0;entryY=0.5;"
  edge="1" parent="..." source="hub" target="target1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="140" y="220"/>
      <mxPoint x="140" y="60"/>
    </Array>
  </mxGeometry>
</mxCell>
```

**Priority order:**
1. Try direction-based entry/exit only (simplest)
2. Add fan-out/fan-in offset points if overlap occurs
3. Add waypoints only when entry/exit alone cannot prevent overlap

---

## CRITICAL: Waypoint Grid Snap Rule

> **All waypoint x,y values MUST be multiples of 10 (end with 0).**

This follows the same grid snap rule as element positions from `PROMPT_OBJECT_FORMAT.md`. Arrow start points, middle waypoints, and end points all obey the 10px grid.

| Point Type | Rule | Valid | Invalid |
|------------|------|-------|---------|
| Start (source exit) | Must end with 0 | `280`, `860`, `1450` | `285`, `865`, `1455` |
| Middle (waypoint) | Must end with 0 | `340`, `990`, `1160` | `345`, `995`, `1155` |
| End (target entry) | Must end with 0 | `250`, `750`, `1020` | `245`, `755`, `1025` |

**Consequence for parallel wire offset:** With the 10px grid constraint, parallel wires in the same channel must be spaced **10px apart** (not 5px). This means each channel band needs 10px per wire.

```
Horizontal channel with 3 parallel wires (10px spacing):
  wire_1: y = 640
  wire_2: y = 650
  wire_3: y = 660

Vertical trunk with 3 parallel wires (10px spacing):
  wire_1: x = 1440
  wire_2: x = 1450
  wire_3: x = 1460
```

---

## CRITICAL: Unique Port Rule

> **On the same element, no two arrows may connect at the same position.**

When multiple arrows connect to one element (either entering or leaving), each arrow MUST use a different connection point (port) on the element boundary. Control this by giving each arrow a different first/last waypoint coordinate so the auto-router selects a different port.

### Source Fan-Out (Multiple Outgoing Arrows)

```
[Element at center (280, 570)] with 2 outgoing arrows:

  conn9:  first waypoint (280, 650)  → exits from bottom center
  conn11: first waypoint (270, 660)  → exits from bottom-left (offset x -10)
                                       ↑ different x = different port
```

| Arrows from Same Source | Solution |
|-------------------------|----------|
| 2 arrows going same direction | Offset first waypoint x or y by ±10px |
| 2 arrows going opposite directions | One goes left, one goes right — naturally different ports |
| 3+ arrows | Each gets unique first waypoint: center, ±10, ±20 |

### Target Fan-In (Multiple Incoming Arrows)

```
[Element at center (260, 220)] with 2 incoming arrows:

  conn1: last waypoint (120, 220)  → enters at center y (from left)
  conn2: last waypoint (130, 250)  → enters at bottom y (from below-left)
                                     ↑ different y = different port
```

| Arrows to Same Target | Solution |
|------------------------|----------|
| 2 arrows from same side | Offset last waypoint y by ±10-30px to enter at different height |
| 2 arrows from different sides | Naturally different ports (one from left, one from top) |

---

## CRITICAL: Zero-Overlap Rule

> **No two arrows may share any horizontal or vertical line segment.**
> Every wire must have its own unique lane for every segment of its path.

This is the core principle of cable management. Overlap = two wires running on top of each other for any length. Crossing (perpendicular intersection) is acceptable and handled by jump arcs.

### How Overlap Happens

With orthogonal routing, DrawIO auto-generates segments between source → waypoint → target. Each segment is either horizontal (fixed y, variable x) or vertical (fixed x, variable y). **Overlap occurs when two different arrows produce segments at the same fixed coordinate with overlapping ranges.**

### Overlap Types and Fixes

#### Type 1: Shared Element Exit

When multiple arrows exit the same element, the auto-router creates segments at the element's center coordinate.

```
PROBLEM — both wires exit at x=280:         FIX — offset one wire's first waypoint:

  [Element center x=280]                     [Element center x=280]
       │  │                                       │      │
       │  │  ← OVERLAP at x=280                  │      │
       │  └──→ target A                      x=280│ x=270│
       └─────→ target B                           │      └──→ target B
                                                  └─────→ target A
```

**Fix:** Give each outgoing wire a different first waypoint x (or y):
- Wire A: first waypoint at **(280, ...)** — exits at element center x
- Wire B: first waypoint at **(270, ...)** — offset by 10px

```xml
<!-- Wire A exits at x=280 (element center) -->
<Array as="points">
  <mxPoint x="280" y="650"/>
  <mxPoint x="300" y="650"/>
</Array>

<!-- Wire B exits at x=270 (offset -10px) -->
<Array as="points">
  <mxPoint x="270" y="645"/>
  <mxPoint x="980" y="645"/>
</Array>
```

#### Type 2: Shared Element Entry

When multiple arrows target the same element, they create overlapping segments approaching it.

```
PROBLEM — both enter at y=220:              FIX — different entry coordinates:

  wire A ──→ y=220 ──→ [Target]             wire A ──→ y=220 ──→ [Target]
  wire B ──→ y=220 ──→ [Target]             wire B ──→ y=250 ──→ [Target]
             ↑ OVERLAP                                   ↑ enters from bottom
```

**Fix:** Route one wire to enter from a different side of the target element:
- Wire A: approaches at **target center y** (enters from left/right)
- Wire B: approaches at **target bottom y** (enters from below)

#### Type 3: Riser Entry Conflicts with Distribution Channel

Vertical risers create horizontal segments when entering/exiting. If a riser entry y matches a distribution channel y, they overlap.

```
PROBLEM — riser and async channel both at y=1005:

  async wire ──────────────────── y=1005     (conn12: x=280 to x=860)
  riser entry ─── y=1005 → riser             (conn18: x=300 to x=340)
                  ↑ OVERLAP at y=1005

FIX — stagger riser entries ABOVE the distribution channel:

  riser entry ─── y=990  → riser             (conn18: safe above async)
  riser entry ─── y=985  → riser             (conn19: 5px offset)
  riser entry ─── y=980  → riser             (conn20: 5px offset)
  async wire  ──────────────────── y=1000    (conn14: below risers)
  async wire  ──────────────────── y=1005    (conn12: separate lane)
  async wire  ──────────────────── y=1010    (conn13: separate lane)
```

**Fix:** Riser entry y-values must be in a different band than distribution channels:
- Distribution channels: `y = gap_center` to `gap_center + N*5`
- Riser entries: `y = gap_center - 10` to `gap_center - N*5` (above)
- Riser exits: `y = next_gap_center` (below, shared is OK if x-ranges don't overlap)

#### Type 4: Long Horizontal Span Conflicts

A wire spanning the full diagram width (e.g., to external services) conflicts with shorter wires at the same y.

```
PROBLEM — conn27 spans full width at y=910, conn28 also at y=910:

  conn27: ──────────────────────────── y=910 (x=420 to x=1465)
  conn28: ────────────────── y=910           (x=980 to x=1445)
          ↑ OVERLAP at y=910, x=980-1445

FIX — offset conn28 to y=920:

  conn27: ──────────────────────────── y=910
  conn28: ────────────────── y=920
```

#### Type 5: Analytics Loop Overlap

When arrows form a chain (A→B→C→D) with elements on the same row, exit and entry vertical segments overlap.

```
PROBLEM — conn29 enters glue at x=520, conn30 exits glue at x=520:

  conn29: ──→ x=520 ↓ [Glue]      (arriving)
  conn30:     x=520 ↑ [Glue] ──→  (leaving)
              ↑ OVERLAP at x=520

FIX — offset exit to x=530:

  conn29: ──→ x=520 ↓ [Glue]      (arriving at center)
  conn30:     x=530 ↑ [Glue] ──→  (leaving offset +10px)
```

---

## Overlap Detection Algorithm

After assigning all waypoints, run this check for every pair of arrows:

### Step 1: Decompose Each Arrow into Segments

For each arrow, trace the full path: source center → wp1 → wp2 → ... → target center.
Between consecutive points, the orthogonal auto-router creates one horizontal + one vertical segment.

```
Point A (x₁, y₁) → Point B (x₂, y₂):
  If exiting source: go vertical first, then horizontal
    Segment 1: V(x₁, y₁→y₂)
    Segment 2: H(y₂, x₁→x₂)
  Between waypoints: go horizontal first, then vertical
    Segment 1: H(y₁, x₁→x₂)
    Segment 2: V(x₂, y₁→y₂)
```

### Step 2: Check All Pairs

For each pair of segments from different arrows:

**Horizontal overlap:** Both segments have the same y. Check if x-ranges intersect.
```
seg_A: y=Y, x from Xa1 to Xa2
seg_B: y=Y, x from Xb1 to Xb2
OVERLAP if: max(Xa1,Xb1) < min(Xa2,Xb2)
```

**Vertical overlap:** Both segments have the same x. Check if y-ranges intersect.
```
seg_A: x=X, y from Ya1 to Ya2
seg_B: x=X, y from Yb1 to Yb2
OVERLAP if: max(Ya1,Yb1) < min(Ya2,Yb2)
```

### Step 3: Fix Each Overlap

Apply the appropriate fix from the overlap types above. After fixing, re-run the check until zero overlaps remain.

---

## Cable Channel Architecture

### Channel Types

```
                    HORIZONTAL DISTRIBUTION CHANNEL (y=constant)
                    ═══════════════════════════════════════════
                    ↑           ↑             ↑           ↑
                    │           │             │           │
    LEFT            │   VERTICAL RISERS       │           │   RIGHT
    VERTICAL        │   (through layers)      │           │   VERTICAL
    TRUNK           │                         │           │   TRUNK
    (inbound)       │                         │           │   (outbound)
    x=120-140       │   x=340,460,580...      │           │   x=1445-1465
                    │           │             │           │
                    ↓           ↓             ↓           ↓
                    ═══════════════════════════════════════════
                    HORIZONTAL DISTRIBUTION CHANNEL (y=constant)
```

| Channel Type | Direction | Purpose | Placement |
|--------------|-----------|---------|-----------|
| **Vertical Trunk** | Top↕Bottom | Main backbone for inbound/outbound traffic | Left and right edges of diagram |
| **Horizontal Distribution** | Left↔Right | Branch-off channel between layers | In the gap between subnet/group rows |
| **Vertical Riser** | Top↕Bottom | Pass-through channel across intermediate layers | In gaps between elements within a layer |

---

## Channel Placement Rules

### 1. Identify Gaps Between Layers

Every diagram with horizontal row layout has gaps between group rows. These gaps are the **horizontal cable channels**.

```
┌─ Subnet A (bottom edge = y₁) ─────────┐
└────────────────────────────────────────┘
  ← GAP: horizontal cable channel zone →    y = (y₁ + y₂) / 2
┌─ Subnet B (top edge = y₂) ────────────┐
└────────────────────────────────────────┘
```

**Calculation:**
```
channel_y = floor((layer_bottom + next_layer_top) / 2 / 10) * 10
```

### 2. Identify Gaps Between Elements (for Vertical Risers)

When arrows must pass **through** an intermediate layer vertically, route them through gaps between elements.

```
┌─ Event Layer ─────────────────────────────────────────┐
│  [elem1]   GAP   [elem2]   GAP   [elem3]   GAP  ... │
│  center    ↑     center    ↑     center    ↑         │
│  x=280   x=340   x=400   x=460   x=520   x=580      │
└───────────────────────────────────────────────────────┘
```

**Calculation:**
```
riser_x = (element_N_center_x + element_N+1_center_x) / 2

With 120px icon spacing (center-to-center):
  riser_x = element_center_x + 60
```

### 3. Lane Allocation (CRITICAL for Zero-Overlap)

Each gap between layers forms a **channel band**. Within each band, allocate individual **lanes** so no two wires share the same y-coordinate. **All lane positions must be multiples of 10.**

```
┌─ Layer A (bottom = 1000) ──────────────────────────┐
│                    (empty space below elements)     │
│  RISER ENTRY BAND:  y=960, 970, 980, 990            │  ← risers enter here (inside layer)
└────────────────────────────────────────────────────┘
  DISTRIBUTION BAND:  y=1000, 1010, 1020               ← horizontal wires here (in gap)
┌─ Layer B (top = 1020) ─────────────────────────────┐
└────────────────────────────────────────────────────┘
```

**Rules:**
- **Riser entries** and **distribution channels** MUST use different y-bands
- Risers enter ABOVE the gap (inside the upper layer's empty space below its elements)
- Distribution channels occupy the gap itself
- Each wire gets a unique y (**10px offset** between parallel wires, grid-aligned)
- With 10px spacing, a 20px gap fits 3 lanes; use empty space inside adjacent layers for overflow

### 4. Source Fan-Out Rule

When N arrows exit the same element, each MUST use a different coordinate for its first segment (see Unique Port Rule above for details):

```
[Element at center (x₀, y₀)] with 3 outgoing arrows:

  Arrow A: first waypoint at (x₀,    y_a)  → exits at element center x
  Arrow B: first waypoint at (x₀-10, y_b)  → exits 10px left
  Arrow C: first waypoint at (x₀+10, y_c)  → exits 10px right
```

All offset values must be multiples of 10. This ensures vertical exit segments never overlap.

### 5. Target Fan-In Rule

When N arrows enter the same element, route them to different sides:

```
[Element at center (x₀, y₀)] with 2 incoming arrows:

  Arrow A: last waypoint approaches y₀       → enters from left (center y)
  Arrow B: last waypoint approaches y₀ + 30  → enters from bottom
```

All offset values must be multiples of 10.

---

## Waypoint Routing Patterns

### Pattern 1: Vertical Trunk (Inbound/Outbound)

For arrows from external actors to internal services through a shared vertical corridor.

```
  [Actor]                    [Target]
   (70,y₁)                  (260,y₂)
     │                          ↑
     └──→ (trunk_x, y₁) ──────→│
              │                  │
              ↓ (trunk_x, y₂) ──┘
```

**Waypoints:** `(trunk_x, source_y), (trunk_x, target_y)`

```xml
<!-- Inbound via left vertical trunk at x=130 -->
<mxCell id="conn-inbound" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;strokeWidth=2;jumpStyle=arc;jumpSize=10;..."
  parent="1" source="staff" target="cloudfront" edge="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="130" y="430"/>
      <mxPoint x="130" y="250"/>
    </Array>
  </mxGeometry>
</mxCell>
```

Note: target entry at y=250 (bottom of element) instead of y=220 (center) to avoid overlapping with another wire entering at y=220.

### Pattern 2: Horizontal Distribution

For arrows that drop from one layer, travel horizontally to a different column, then drop to the next layer.

```
  [Source]                         [Target]
   (x₁, y₁)                       (x₂, y₃)
     │                                ↑
     ↓                                │
   (x₁, channel_y) ──────→ (x₂, channel_y)
                                      │
                                      ↓
```

**Waypoints:** `(source_x, channel_y), (target_x, channel_y)`

```xml
<!-- Horizontal distribution at y=640, unique lane -->
<mxCell id="conn-distribute" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;strokeWidth=2;jumpStyle=arc;jumpSize=10;..."
  parent="1" source="ibank-svc" target="transfer-svc" edge="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="400" y="640"/>
      <mxPoint x="860" y="640"/>
    </Array>
  </mxGeometry>
</mxCell>
```

### Pattern 3: Vertical Riser (Through Intermediate Layer)

For arrows that must pass through a layer they don't connect to. **Riser entry y MUST differ from distribution channel y.**

```
  [Source in Layer A]
   (x₁, y₁)
     │
     ↓
   (x₁, entry_y) ──→ (riser_x, entry_y)     ← riser entry band (y=970-990)
                          │
                          │  (passes through Layer B)
                          │                            distribution band (y=1000-1010)
                          │                            ← separate, no conflict
                       (riser_x, exit_y)  ──→ (x₂, exit_y)  ← riser exit
                                                   │
                                                   ↓
                                             [Target in Layer C]
```

**Waypoints:** `(riser_x, entry_y), (riser_x, exit_y)`

Stagger entry y per riser to prevent horizontal overlap:

```xml
<!-- Riser 1: entry at y=990 (topmost lane in riser band) -->
<Array as="points">
  <mxPoint x="340" y="990"/>
  <mxPoint x="340" y="1160"/>
</Array>

<!-- Riser 2: entry at y=980 (offset -10px, grid-aligned) -->
<Array as="points">
  <mxPoint x="460" y="980"/>
  <mxPoint x="460" y="1160"/>
</Array>

<!-- Riser 3: entry at y=970 (offset -20px, grid-aligned) -->
<Array as="points">
  <mxPoint x="580" y="970"/>
  <mxPoint x="580" y="1160"/>
</Array>
```

### Pattern 4: Right-Side Trunk (Multi-Hop to External)

For arrows traveling from internal services to external/security services on the right side.

```
  [Internal Service]          [External Service]
   (x₁, y₁)                    (x₃, y₃)
     │                             ↑
     └──→ (trunk_x, y_offset)     │       ← y_offset ≠ y₁ if another wire uses y₁
              │                    │
              ↓ (trunk_x, y₃) ────┘
```

**Important:** When multiple wires exit at the same y from their source, offset the first waypoint y to create separate horizontal lanes:

```xml
<!-- Wire A: exits at source y=760, horizontal at y=750 (offset -10px) -->
<Array as="points">
  <mxPoint x="1460" y="750"/>
  <mxPoint x="1460" y="800"/>
</Array>

<!-- Wire B: exits at source y=760, horizontal at y=770 (offset +10px) -->
<Array as="points">
  <mxPoint x="980" y="770"/>
  <mxPoint x="1455" y="770"/>
  <mxPoint x="1455" y="800"/>
</Array>
```

### Pattern 5: Chain/Loop (Same-Row Elements)

When arrows form a chain between elements on the same row (e.g., A→B→C), exit and entry segments MUST use different x-coordinates.

```
WRONG — exit and entry both at x=520:     CORRECT — offset exit to x=530:

  conn29 ──→ x=520 ↓ [Glue]               conn29 ──→ x=520 ↓ [Glue]
  conn30     x=520 ↑ [Glue] ──→           conn30     x=530 ↑ [Glue] ──→
             ↑ OVERLAP                                 ↑ separate lanes
```

```xml
<!-- Arriving at glue: x=520 (element center) -->
<Array as="points">
  <mxPoint x="310" y="1515"/>
  <mxPoint x="520" y="1515"/>
</Array>

<!-- Leaving glue: x=530 (offset +10px from center) -->
<Array as="points">
  <mxPoint x="530" y="1520"/>
  <mxPoint x="280" y="1520"/>
</Array>
```

---

## Absolute Position Calculation

Waypoints use **absolute coordinates** (relative to the root parent, id="1"). To calculate the absolute position of an element inside nested groups:

```
absolute_x = parent1_x + parent2_x + ... + element_x + (element_width / 2)
absolute_y = parent1_y + parent2_y + ... + element_y + (element_height / 2)
```

### Example

```
AWS Cloud:     x=160, y=80
  Region:      x=20,  y=40   (relative to Cloud)
    VPC:       x=20,  y=200  (relative to Region)
      Subnet:  x=20,  y=40   (relative to VPC)
        Icon:  x=30,  y=30   (relative to Subnet), size=60x60

Icon center (absolute):
  x = 160 + 20 + 20 + 20 + 30 + 30 = 280
  y = 80  + 40 + 200 + 40 + 30 + 30 = 420
```

---

## Channel Planning Workflow

### Step 1: Map All Element Absolute Positions

Calculate absolute center positions for every element that has connections.

### Step 2: List All Connections

For each arrow, record: source center, target center, arrow type (sync/async/DB/etc).

### Step 3: Identify Layer Gaps

Find the vertical gaps between each row of groups/subnets. These become horizontal distribution channel bands.

### Step 4: Assign Vertical Trunks

| Trunk | X Position | Purpose |
|-------|------------|---------|
| Left inbound | `x = cloud_left - 40` to `cloud_left - 20` | External actors entering diagram |
| Right outbound | `x = vpc_right + 10` to `vpc_right + 30` | Connections to external services |

Assign each wire in the trunk a unique x-offset (10px apart).

### Step 5: Assign Vertical Risers

For each arrow that must cross an intermediate layer:

```
riser_x = midpoint between elements in the intermediate layer
```

Each riser gets a unique x position. No two risers share the same x.

### Step 6: Assign Horizontal Lanes (Per Channel Band)

For each gap between layers, create two bands:

```
RISER ENTRY BAND:    y = gap_start - 5*N  (stagger upward from gap)
DISTRIBUTION BAND:   y = gap_center + 5*N  (stagger within gap)
```

Each wire gets a unique y within its band.

### Step 7: Apply Fan-Out/Fan-In Offsets

For elements with multiple outgoing or incoming arrows, offset first/last waypoints by ±10px.

### Step 8: Verify Zero Overlaps

Decompose every arrow into segments. Check all pairs for shared horizontal lines (same y, overlapping x) or shared vertical lines (same x, overlapping y). Fix any found.

---

## Crossing Behavior

### Jump Arc (Default)

When two wires cross perpendicularly (horizontal meets vertical), `jumpStyle=arc` draws a small semicircle, making it clear they are separate paths (not a junction).

```
         │
    ─────╭╮────    (arc jump at perpendicular crossing)
         │
```

| Property | Value | Description |
|----------|-------|-------------|
| `jumpStyle` | `arc` | Semicircle arc at crossings |
| `jumpSize` | `10` | 10px radius arc |

### Crossing vs Overlap

| Situation | Visual | Acceptable? |
|-----------|--------|-------------|
| **Crossing** — H meets V at a point | Arc jump shown | Yes (unavoidable in complex diagrams) |
| **Overlap** — two H segments at same y | Lines merge, invisible | **NO — must fix with offset** |
| **Overlap** — two V segments at same x | Lines merge, invisible | **NO — must fix with offset** |

---

## Complete Example: Banking Architecture Channels

### Channel Map

```
                    x=120  x=130  x=140                    x=1445 x=1450 x=1455 x=1460 x=1465
                      │      │      │                         │      │      │      │      │
  [Customers] ────────┘      │      │     ┌─ Edge ─────────┐  │      │      │      │      │
  [Staff] ───────────────────┘      │     │ CF  WAF  R53   │  │      │      │      │      │
  [Partners] ───────────────────────┘     └────────────────┘  │      │      │      │      │
                                    y=325 ═══════════════════  │      │      │      │      │
                                    y=330 ═══════════════════  │      │      │      │      │
                                          ┌─ Public ────────┐  │      │      │      │      │
                                          │ ALB(x=280)      │  │      │      │      │      │
                                          │     ALB(x=290)  │  │      │      │      │      │
                                          │       ← dual    │  │      │      │      │      │
                                          │        approach  │  │      │      │      │      │
                                          └─────────────────┘  │      │      │      │      │
                                    y=495 ═══════════════════  │      │      │      │      │
                                    y=500 ═══════════════════  │      │      │      │      │
                                          ┌─ Channel ───────┐  │      │      │      │      │
                                          │ Mob  IBank Auth  │──│──────│──────│──────┘      │
                                          └─────────────────┘  │      │    [Cognito]        │
                                  DISTRIBUTION BAND (x10 grid): │      │                     │
                                    y=640 ═══ chan→core 1 ═════ │      │                     │
                                    y=650 ═══ chan→core 2 ═════ │      │                     │
                                    y=660 ═══ chan→core 3 ═════ │      │                     │
                                    y=670 ═══ storage chan ═════ │      │                     │
                                          ┌─ Core ──────────┐  │      │                     │
                                          │ Acct  Txn  Loan  │──│──────│─────────────────────┘
                                          │ Card  Fraud      │  │    [SWIFT] [PayNet]
                                          └─────────────────┘  │    [CreditBureau]
                                  RISER ENTRY BAND (x10 grid):  │
                                    y=950 ═══ riser 5 (conn22) │═══════════════════════
                                    y=960 ═══ riser 4 (conn21) │═══════════════════════
                                    y=970 ═══ riser 3 (conn20) │═══════════════════════
                                    y=980 ═══ riser 2 (conn19) │═══════════════════════
                                    y=990 ═══ riser 1 (conn18) │═══════════════════════
                                  DISTRIBUTION BAND (x10 grid): │
                                    y=1000 ═══ async 1 (conn14)│═══════════════════════
                                    y=1010 ═══ async 2 (conn12)│═══════════════════════
                                    y=1020 ═══ async 3 (conn13)│═══════════════════════
                                          ┌─ Event ─────────┐  │
                                          │ SQS SNS EB SF λ  │  │
                                  risers: │ ↕  ↕  ↕  ↕  ↕   │  │
                                  x=340─580,700,820           │  │
                                          └─────────────────┘  │
                                  RISER EXIT BAND:              │
                                    y=1160 ═══ all risers ═════│═══════════════════════
                                          ┌─ Data ──────────┐  │
                                          │ RDS DDB Cache S3 │  │
                                          └─────────────────┘  │
                                    y=1510 ═══ analytics ══════│═══════════════════════
                                    y=1520 ═══ analytics ══════│═══════════════════════
                                    y=1530 ═══ analytics ══════│═══════════════════════
                                          ┌─ Analytics ─────┐  │
                                          │ RS Athena Glue QS│  │
                                          └─────────────────┘  │
```

### Channel Assignments (Zero-Overlap)

| Channel | Type | Position | Wire | Notes |
|---------|------|----------|------|-------|
| Left trunk 1 | Vertical | x=120 | conn1: customers → cloudfront | |
| Left trunk 2 | Vertical | x=130 | conn2: staff → cloudfront | enters at y=250 (bottom) |
| Left trunk 3 | Vertical | x=140 | conn3: partners → apigw | |
| Edge→Public 1 | Horizontal | y=320 | conn4: cloudfront→alb | approach at x=280 |
| Edge→Public 2 | Horizontal | y=330 | conn5: waf→alb | approach at x=290 (offset) |
| Public→Channel 1 | Horizontal | y=490 | conn8: apigw→auth | |
| Public→Channel 2 | Horizontal | y=500 | conn7: alb→ibank | exit ALB at x=290 |
| Chan→Core 1 | Horizontal | y=640 | conn10: ibank→transfer | |
| Chan→Core 2 | Horizontal | y=650 | conn9: mobile→account | exit mobile at x=280 |
| Chan→Core 3 | Horizontal | y=660 | conn11: mobile→payment | exit mobile at x=270 |
| Storage | Horizontal | y=670 | conn23: notif→s3-docs | via left channel x=250 |
| Riser entry 1 | Horizontal | y=990 | conn18: →riser x=340 | |
| Riser entry 2 | Horizontal | y=980 | conn19: →riser x=460 | exit at x=850 |
| Riser entry 3 | Horizontal | y=970 | conn20: →riser x=580 | |
| Riser entry 4 | Horizontal | y=960 | conn21: →riser x=700 | |
| Riser entry 5 | Horizontal | y=580 | conn22: →riser x=820 | via intermediate at y=580 |
| Core→Event 1 | Horizontal | y=1000 | conn14: notif→sqs-notif | |
| Core→Event 2 | Horizontal | y=1010 | conn12: transfer→sqs-txn | |
| Core→Event 3 | Horizontal | y=1020 | conn13: payment→sqs-payment | exit at x=970 |
| All riser exits | Horizontal | y=1160 | conn18-22 exit | non-overlapping x-ranges |
| DB Riser 1 | Vertical | x=340 | conn18: account→rds-core | |
| DB Riser 2 | Vertical | x=460 | conn19: transfer→rds-txn | |
| DB Riser 3 | Vertical | x=580 | conn20: loan→rds-loan | |
| DB Riser 4 | Vertical | x=700 | conn21: card→dynamodb | |
| DB Riser 5 | Vertical | x=820 | conn22: auth→elasticache | |
| Right trunk 1 | Vertical | x=1440 | conn28: fraud→fraud-ext | at y=920 |
| Right trunk 2 | Vertical | x=1450 | conn24: auth→cognito | exit at y=560 |
| Right trunk 3 | Vertical | x=1460 | conn26: payment→paynet | via y=770 |
| Right trunk 4 | Vertical | x=1470 | conn25: transfer→swift | via y=750 |
| Right trunk 5 | Vertical | x=1480 | conn27: credit→bureau | |
| Analytics 1 | Horizontal | y=1510 | conn29: rds-core→glue | exit at x=310 |
| Analytics 2 | Horizontal | y=1520 | conn30: glue→redshift | exit at x=530 |
| Analytics 3 | Horizontal | y=1530 | conn31: redshift→QS | exit at x=290 |

---

## Validation Checklist

### Entry/Exit Point Checks

| Check | Rule |
|-------|------|
| ☐ | Fan-out arrows use spread exit points (`0.25`, `0.5`, `0.75`) — no two arrows exit same point |
| ☐ | Fan-in arrows use spread entry points (`0.25`, `0.5`, `0.75`) — no two arrows enter same point |
| ☐ | Diagonal arrows use perpendicular face pairs (e.g., exit bottom → enter left) |
| ☐ | Cross-container arrows use offset entry points (`0.25` or `0.75`) to avoid overlap with direct arrows |
| ☐ | Cycle arrows alternate auto-routed and explicitly routed segments |
| ☐ | Corner points `(0,0)`, `(1,0)`, `(0,1)`, `(1,1)` are NOT used (they cause awkward routing) |
| ☐ | Direct horizontal/vertical arrows use auto-routing (no unnecessary entry/exit overrides) |

### Waypoint Cable Management Checks

| Check | Rule |
|-------|------|
| ☐ | All edges use `edgeStyle=orthogonalEdgeStyle;rounded=0` |
| ☐ | All edges have `jumpStyle=arc;jumpSize=10` |
| ☐ | **ZERO horizontal overlaps** — no two arrows share the same y with overlapping x-range |
| ☐ | **ZERO vertical overlaps** — no two arrows share the same x with overlapping y-range |
| ☐ | Riser entry y-values are in a separate band from distribution channel y-values |
| ☐ | Each element with multiple outgoing arrows uses different exit coordinates (fan-out) |
| ☐ | Each element with multiple incoming arrows uses different entry coordinates (fan-in) |
| ☐ | Vertical risers pass through gaps between elements (not over them) |
| ☐ | Waypoint coordinates are in absolute position (parent="1" coordinate space) |
| ☐ | Each riser has a unique x-position |
| ☐ | Each trunk wire has a unique x-position |
| ☐ | No arrow has more than 4 waypoints (keep routing simple) |
| ☐ | Straight vertical/horizontal arrows have NO waypoints (auto-route is fine) |
| ☐ | **All waypoint x,y values are multiples of 10** (grid snap rule) |
| ☐ | **No two arrows share the same connection point on any element** (unique port rule) |

---

## Quick Reference

```
LEVEL 1 — Entry/Exit Points (try first):
  Fan-out exit:      Spread across face: (1,0.25), (1,0.5), (1,0.75)
  Fan-in entry:      Spread across face: (0,0.25), (0,0.5), (0,0.75)
  Diagonal routing:  Perpendicular face pairs (exit bottom → enter left)
  Cross-container:   Offset entry to 0.25 or 0.75 to avoid overlap
  Cycle routing:     Alternate auto + explicit (exit 0.5,1 → enter 1,0.5)
  Skip if:           Direct horizontal/vertical, simple bidirectional

LEVEL 2 — Waypoint Cable Management (complex diagrams):
  Edge style:        edgeStyle=orthogonalEdgeStyle;rounded=0
  Jump arcs:         jumpStyle=arc;jumpSize=10
  Parallel offset:   10px (all directions, must be x10 grid)
  Max waypoints:     2-4 per arrow
  Trunk placement:   Left side (inbound), Right side (outbound)
  Riser placement:   Midpoint between elements in intermediate layer
  Riser entry band:  ABOVE the gap (inside upper layer empty space)
  Distribution band: IN the gap between layers
  Fan-out rule:      ±10px offset per additional wire from same element
  Zero-overlap:      MANDATORY — every segment must have unique lane
  Position calc:     Sum all parent x/y + element x/y + size/2
```
