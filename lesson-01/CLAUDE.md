# CLAUDE.md - DrawIO Cloud Architecture Diagram Guidelines

## Overview
When creating `.drawio` files for cloud architecture diagrams, always use the correct shape library icons for each cloud provider. DrawIO uses XML format with specific style attributes to reference official cloud service shapes.

## File Structure
DrawIO files are XML-based with this basic structure:
```xml
<mxfile host="app.diagrams.net">
  <diagram name="Page-1" id="...">
    <mxGraphModel>
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- Your shapes go here -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## CRITICAL: Parent-Child Relationships for Grouping

**For elements to move together when dragging a group container, child elements MUST have their `parent` attribute set to the container's ID.**

### How It Works
- `parent="0"` - The root cell (id="0")
- `parent="1"` - Direct child of the diagram root (top-level elements)
- `parent="<container-id>"` - Child of a specific container (moves with container)

### Hierarchy Example
```
AWS Cloud (parent="1")
  └── Region (parent="aws-cloud")
        └── VPC (parent="region")
              ├── Public Subnet (parent="vpc")
              │     ├── IGW (parent="public-subnet")
              │     └── ALB (parent="public-subnet")
              └── Private Subnet (parent="vpc")
                    ├── ECS (parent="private-subnet")
                    └── RDS (parent="private-subnet")
```

### Coordinate System
**IMPORTANT:** When an element has a parent container, its `x` and `y` coordinates are **relative to the parent's top-left corner**, not the diagram origin.

```xml
<!-- Container at absolute position (200, 100) -->
<mxCell id="vpc" parent="1" ...>
  <mxGeometry x="200" y="100" width="400" height="300"/>
</mxCell>

<!-- Child at (20, 40) RELATIVE to VPC = absolute (220, 140) -->
<mxCell id="ecs" parent="vpc" ...>
  <mxGeometry x="20" y="40" width="60" height="60"/>
</mxCell>
```

### Nesting Rules and Z-Order

**XML Order determines visual stacking - elements defined later appear in front.**

**XML Definition Order (top to bottom in file):**
```
1. Groups/Containers (AWS Cloud → Region → VPC → Subnets)
2. Shapes/Service Icons (inside their parent containers)
3. Text Labels (standalone text boxes)
4. Edges/Arrows (connection lines between shapes)
```

**Visual Z-Order (back to front):**
| Layer | Element Type | XML Position |
|-------|--------------|--------------|
| 1 (Back) | Top-level groups (AWS Cloud, On-Prem DC) | First |
| 2 | Sub-groups (Region, VPC) | After parent |
| 3 | Leaf groups (Subnets, Zones) | After parent |
| 4 | Service icons/shapes | After their container |
| 5 | Text labels | After shapes |
| 6 (Front) | Edges/Arrows | Last |

**Example XML Structure:**
```xml
<!-- 1. Groups (back) - outermost to innermost -->
<mxCell id="aws-cloud" value="AWS Cloud" ... />
  <mxCell id="region" parent="aws-cloud" ... />
    <mxCell id="vpc" parent="region" ... />
      <mxCell id="subnet" parent="vpc" ... />

<!-- 2. Shapes (middle) - inside containers -->
        <mxCell id="ecs" parent="subnet" ... />
        <mxCell id="rds" parent="subnet" ... />

<!-- 3. Text labels -->
<mxCell id="label-1" value="Security Rules" ... />

<!-- 4. Edges (front) - always last, always parent="1" -->
<mxCell id="edge-1" parent="1" source="ecs" target="rds" edge="1" />
```

**Rules:**
- **Edges must have `parent="1"`** - They connect across containers and need to be at root level
- **Shapes use relative coordinates** - Child coordinates are relative to parent's top-left
- **Container must have `vertex="1"`** - Required to be a valid parent for nesting

---

## AWS Shape References

Use `shape=mxgraph.aws4.` prefix for AWS Architecture 2024 icons.

### Compute
| Service | Shape Style |
|---------|-------------|
| EC2 | `shape=mxgraph.aws4.ec2` |
| Lambda | `shape=mxgraph.aws4.lambda_function` |
| ECS | `shape=mxgraph.aws4.ecs` |
| EKS | `shape=mxgraph.aws4.eks` |
| Fargate | `shape=mxgraph.aws4.fargate` |
| Elastic Beanstalk | `shape=mxgraph.aws4.elastic_beanstalk` |

### Storage
| Service | Shape Style |
|---------|-------------|
| S3 | `shape=mxgraph.aws4.s3` |
| EBS | `shape=mxgraph.aws4.elastic_block_store` |
| EFS | `shape=mxgraph.aws4.elastic_file_system` |
| Glacier | `shape=mxgraph.aws4.glacier` |

### Database
| Service | Shape Style |
|---------|-------------|
| RDS | `shape=mxgraph.aws4.rds` |
| DynamoDB | `shape=mxgraph.aws4.dynamodb` |
| Aurora | `shape=mxgraph.aws4.aurora` |
| ElastiCache | `shape=mxgraph.aws4.elasticache` |
| Redshift | `shape=mxgraph.aws4.redshift` |

### Networking
| Service | Shape Style |
|---------|-------------|
| VPC | `shape=mxgraph.aws4.vpc` |
| CloudFront | `shape=mxgraph.aws4.cloudfront` |
| Route 53 | `shape=mxgraph.aws4.route_53` |
| API Gateway | `shape=mxgraph.aws4.api_gateway` |
| ELB/ALB | `shape=mxgraph.aws4.elastic_load_balancing` |
| NAT Gateway | `shape=mxgraph.aws4.nat_gateway` |
| Internet Gateway | `shape=mxgraph.aws4.internet_gateway` |

### Security
| Service | Shape Style |
|---------|-------------|
| IAM | `shape=mxgraph.aws4.iam` |
| Cognito | `shape=mxgraph.aws4.cognito` |
| KMS | `shape=mxgraph.aws4.key_management_service` |
| WAF | `shape=mxgraph.aws4.waf` |
| Secrets Manager | `shape=mxgraph.aws4.secrets_manager` |

### Messaging & Integration
| Service | Shape Style |
|---------|-------------|
| SQS | `shape=mxgraph.aws4.sqs` |
| SNS | `shape=mxgraph.aws4.sns` |
| EventBridge | `shape=mxgraph.aws4.eventbridge` |
| Step Functions | `shape=mxgraph.aws4.step_functions` |

### Monitoring
| Service | Shape Style |
|---------|-------------|
| CloudWatch | `shape=mxgraph.aws4.cloudwatch` |
| X-Ray | `shape=mxgraph.aws4.xray` |

### AWS Groups (for VPC, Subnets, etc.)
| Group Type | Shape Style |
|------------|-------------|
| AWS Cloud | `shape=mxgraph.aws4.group;grIcon=mxgraph.aws4.group_aws_cloud` |
| Region | `shape=mxgraph.aws4.group;grIcon=mxgraph.aws4.group_region` |
| VPC | `shape=mxgraph.aws4.group;grIcon=mxgraph.aws4.group_vpc` |
| Availability Zone | `shape=mxgraph.aws4.group;grIcon=mxgraph.aws4.group_availability_zone` |
| Public Subnet | `shape=mxgraph.aws4.group;grIcon=mxgraph.aws4.group_security_group` (green) |
| Private Subnet | `shape=mxgraph.aws4.group;grIcon=mxgraph.aws4.group_security_group` (blue) |

---

## Azure Shape References

Use `shape=mxgraph.azure.` prefix for Azure icons.

### Compute
| Service | Shape Style |
|---------|-------------|
| Virtual Machine | `shape=mxgraph.azure.compute.virtual_machine` |
| App Service | `shape=mxgraph.azure.compute.app_services` |
| Functions | `shape=mxgraph.azure.compute.function_apps` |
| AKS | `shape=mxgraph.azure.compute.kubernetes_services` |
| Container Instances | `shape=mxgraph.azure.compute.container_instances` |

### Storage
| Service | Shape Style |
|---------|-------------|
| Blob Storage | `shape=mxgraph.azure.storage.blob_block` |
| Storage Account | `shape=mxgraph.azure.storage.storage_accounts` |
| Disk | `shape=mxgraph.azure.storage.disks` |

### Database
| Service | Shape Style |
|---------|-------------|
| SQL Database | `shape=mxgraph.azure.databases.sql_databases` |
| Cosmos DB | `shape=mxgraph.azure.databases.azure_cosmos_db` |
| Redis Cache | `shape=mxgraph.azure.databases.cache_redis` |

### Networking
| Service | Shape Style |
|---------|-------------|
| Virtual Network | `shape=mxgraph.azure.networking.virtual_networks` |
| Load Balancer | `shape=mxgraph.azure.networking.load_balancers` |
| Application Gateway | `shape=mxgraph.azure.networking.application_gateways` |
| CDN | `shape=mxgraph.azure.networking.cdn_profiles` |
| DNS Zone | `shape=mxgraph.azure.networking.dns_zones` |

### Security
| Service | Shape Style |
|---------|-------------|
| Key Vault | `shape=mxgraph.azure.security.key_vaults` |
| Azure AD | `shape=mxgraph.azure.identity.azure_active_directory` |

---

## Kubernetes Shape References

Use `shape=mxgraph.kubernetes.` prefix for Kubernetes icons.

| Resource | Shape Style |
|----------|-------------|
| Pod | `shape=mxgraph.kubernetes.pod` |
| Deployment | `shape=mxgraph.kubernetes.deploy` |
| Service | `shape=mxgraph.kubernetes.svc` |
| Ingress | `shape=mxgraph.kubernetes.ing` |
| ConfigMap | `shape=mxgraph.kubernetes.cm` |
| Secret | `shape=mxgraph.kubernetes.secret` |
| PersistentVolume | `shape=mxgraph.kubernetes.pv` |
| PersistentVolumeClaim | `shape=mxgraph.kubernetes.pvc` |
| StatefulSet | `shape=mxgraph.kubernetes.sts` |
| DaemonSet | `shape=mxgraph.kubernetes.ds` |
| ReplicaSet | `shape=mxgraph.kubernetes.rs` |
| Namespace | `shape=mxgraph.kubernetes.ns` |
| Node | `shape=mxgraph.kubernetes.node` |
| Cluster | `shape=mxgraph.kubernetes.control_plane` |

---

## Lines and Edges (Connections)

Edges connect shapes and represent data flow, dependencies, or relationships between components.

### Edge Structure

```xml
<mxCell id="edge-1"
  style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#232F3E;strokeWidth=2;"
  edge="1" parent="1" source="source-id" target="target-id">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### Edge Style Attributes

| Attribute | Values | Description |
|-----------|--------|-------------|
| `edgeStyle` | `orthogonalEdgeStyle`, `elbowEdgeStyle`, `entityRelationEdgeStyle`, `none` | Routing algorithm |
| `curved` | `0`, `1` | Enable curved lines |
| `rounded` | `0`, `1` | Round corners on orthogonal edges |
| `strokeColor` | `#RRGGBB` | Line color |
| `strokeWidth` | `1`, `2`, `3`... | Line thickness in pixels |
| `dashed` | `0`, `1` | Solid or dashed line |
| `dashPattern` | `8 8`, `4 4` | Custom dash pattern (dash space) |
| `opacity` | `0-100` | Line transparency |

### Edge Routing Styles

| Style | Description | Use Case |
|-------|-------------|----------|
| `orthogonalEdgeStyle` | Right-angle turns only | Clean architecture diagrams |
| `elbowEdgeStyle` | Single elbow bend | Simple L-shaped connections |
| `entityRelationEdgeStyle` | ER diagram style | Database relationships |
| `none` | Direct straight line | Point-to-point connections, dense layouts |

### Choosing Edge Style for Dense Layouts

**When multiple complex shapes are close together, use straight lines (`edgeStyle=none`) to avoid crossings:**

```
Orthogonal (may cross):        Straight (direct path):
    ┌───┐     ┌───┐                ┌───┐     ┌───┐
    │ A │──┐  │ C │                │ A │╲    │ C │
    └───┘  │  └───┘                └───┘ ╲   └───┘
           │                              ╲
    ┌───┐  │  ┌───┐                ┌───┐  ╲  ┌───┐
    │ B │──┴──│ D │                │ B │───╲─│ D │
    └───┘     └───┘                └───┘    ╲└───┘
```

**When to use straight lines:**
- Dense shape clusters where orthogonal routing creates crossings
- Short connections between adjacent shapes
- Diagonal relationships in grid layouts

**Straight line example:**
```xml
<!-- Direct straight line for dense layout -->
<mxCell style="edgeStyle=none;rounded=0;strokeColor=#232F3E;strokeWidth=2;endArrow=classic;endFill=1;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### Line Style Examples

```xml
<!-- Solid orthogonal line -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#232F3E;strokeWidth=2;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Dashed line (async/optional flow) -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;dashed=1;dashPattern=8 8;strokeColor=#666666;strokeWidth=1;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Curved line -->
<mxCell style="curved=1;strokeColor=#232F3E;strokeWidth=2;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

---

## Arrows (Edge Endpoints)

Arrow styles define the visual indicators at the start and end of edges.

### Arrow Attributes

| Attribute | Description |
|-----------|-------------|
| `startArrow` | Arrow shape at source end |
| `endArrow` | Arrow shape at target end |
| `startFill` | `0` = outline only, `1` = filled |
| `endFill` | `0` = outline only, `1` = filled |
| `startSize` | Arrow size at source (default: 6) |
| `endSize` | Arrow size at target (default: 6) |

### Arrow Types

| Arrow Type | Style Value | Description |
|------------|-------------|-------------|
| Classic | `classic` | Standard filled arrow |
| Open | `open` | Open arrow (V-shape) |
| Block | `block` | Filled block arrow |
| Oval | `oval` | Circular endpoint |
| Diamond | `diamond` | Diamond shape |
| Diamond (outline) | `diamondThin` | Thin diamond outline |
| None | `none` | No arrow |
| Circle | `circle` | Small circle |
| ERone | `ERone` | ER diagram "one" |
| ERmany | `ERmany` | ER diagram "many" (crow's foot) |
| ERoneToMany | `ERoneToMany` | ER "one-to-many" |
| ERzeroToMany | `ERzeroToMany` | ER "zero-to-many" |
| ERzeroToOne | `ERzeroToOne` | ER "zero-to-one" |
| Async | `async` | Half arrow (async message) |

### Arrow Type Selection Guide

**Choose arrow type based on connection semantics:**

| Connection Type | Arrow | Style | Example |
|-----------------|-------|-------|---------|
| **HTTP Request/Response** | `classic` | `endArrow=classic;endFill=1` | Users→ALB, ALB→Service |
| **Data Flow** | `classic` | `endArrow=classic;endFill=1;flowAnimation=1` | Service→Database |
| **Read/Write (bidirectional)** | Both `classic` | `startArrow=classic;startFill=1;endArrow=classic;endFill=1` | App↔RDS, App↔Redis |
| **Runs On / Lightweight** | `open` | `endArrow=open;endFill=0` | Service→Fargate, Cluster→CloudWatch |
| **Async Messaging (poll)** | `async` | `endArrow=async;endFill=1` | Worker→SQS |
| **Pull / Fetch** | `block` | `endArrow=block;endFill=1` | ECS←ECR (image pull) |
| **Publish / Push** | `classic` | `endArrow=classic;endFill=1` | Service→SQS |
| **Association (no direction)** | `none` | `endArrow=none` | Dashed grouping lines |

### Quick Reference

```
classic  ────▶     Primary data flow, HTTP, publish
open     ────>     Lightweight, runs-on, metrics
async    ────▷     Async polling, message consumption
block    ────▮     Pull/fetch operations
both     ◀───▶     Bidirectional read/write
none     ─────     Association, no direction
```

### Arrow Examples

```xml
<!-- Standard directional arrow (source to target) -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;endArrow=classic;endFill=1;startArrow=none;strokeWidth=2;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Bidirectional arrow -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;startArrow=classic;startFill=1;endArrow=classic;endFill=1;strokeWidth=2;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Open arrow (lightweight) -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;endArrow=open;endFill=0;strokeWidth=1;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Async message arrow -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;endArrow=async;endFill=1;dashed=1;strokeWidth=1;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Database relationship (one-to-many) -->
<mxCell style="edgeStyle=entityRelationEdgeStyle;startArrow=ERone;endArrow=ERmany;strokeWidth=1;"
  edge="1" parent="1" source="table1" target="table2">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### Edge Labels

Add labels to edges using the `value` attribute or child `mxCell`:

```xml
<!-- Simple edge label -->
<mxCell id="edge-1" value="HTTPS"
  style="edgeStyle=orthogonalEdgeStyle;endArrow=classic;endFill=1;fontSize=10;fontColor=#666666;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Positioned edge label -->
<mxCell id="edge-1" style="edgeStyle=orthogonalEdgeStyle;endArrow=classic;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry">
    <mxPoint as="offset"/>
  </mxGeometry>
</mxCell>
<mxCell id="edge-1-label" value="API Call" style="edgeLabel;fontSize=10;"
  vertex="1" connectable="0" parent="edge-1">
  <mxGeometry x="0.5" relative="1" as="geometry"/>
</mxCell>
```

---

## Frames and Containers

Frames are rectangular containers used to group related components visually.

### Basic Frame Structure

```xml
<mxCell id="frame-1" value="Frame Title"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=none;strokeColor=#000000;strokeWidth=2;dashed=0;verticalAlign=top;align=left;spacingLeft=10;spacingTop=5;fontSize=14;fontStyle=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="40" width="300" height="200" as="geometry"/>
</mxCell>
```

### Frame Style Attributes

| Attribute | Values | Description |
|-----------|--------|-------------|
| `rounded` | `0` | **Always use square corners for groups** |
| `fillColor` | `#RRGGBB`, `none` | Background color |
| `strokeColor` | `#RRGGBB` | Border color |
| `strokeWidth` | `1`, `2`, `3`... | Border thickness |
| `dashed` | `0`, `1` | Solid or dashed border |
| `opacity` | `0-100` | Fill transparency |
| `shadow` | `0`, `1` | Drop shadow effect |
| `glass` | `0`, `1` | Glass/gradient effect |

### Frame Title Positioning

| Attribute | Values | Description |
|-----------|--------|-------------|
| `verticalAlign` | `top`, `middle`, `bottom` | Vertical text position |
| `align` | `left`, `center`, `right` | Horizontal text position |
| `spacingLeft` | pixels | Left padding for text |
| `spacingTop` | pixels | Top padding for text |
| `labelPosition` | `left`, `center`, `right` | Label horizontal position |
| `verticalLabelPosition` | `top`, `middle`, `bottom` | Label vertical position |

### Common Frame Styles

**Note: Group/container shapes should always use `rounded=0` (square corners).**

```xml
<!-- Simple border frame -->
<mxCell id="frame-simple" value="Component Group"
  style="rounded=0;fillColor=none;strokeColor=#333333;strokeWidth=1;verticalAlign=top;align=left;spacingLeft=10;spacingTop=5;fontStyle=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="40" width="300" height="200" as="geometry"/>
</mxCell>

<!-- Frame with background -->
<mxCell id="frame-bg" value="Service Layer"
  style="rounded=0;fillColor=#f5f5f5;strokeColor=#666666;strokeWidth=2;verticalAlign=top;align=left;spacingLeft=15;spacingTop=8;fontStyle=1;fontSize=14;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="40" width="300" height="200" as="geometry"/>
</mxCell>

<!-- Dashed boundary frame -->
<mxCell id="frame-dashed" value="Optional Components"
  style="rounded=0;fillColor=none;strokeColor=#999999;strokeWidth=1;dashed=1;dashPattern=8 4;verticalAlign=top;align=left;spacingLeft=10;fontStyle=2;fontColor=#666666;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="40" width="300" height="200" as="geometry"/>
</mxCell>

<!-- Swimlane (horizontal partition) -->
<mxCell id="swimlane-1" value="Frontend Layer"
  style="swimlane;horizontal=1;rounded=0;fillColor=#dae8fc;strokeColor=#6c8ebf;startSize=30;fontStyle=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="40" width="600" height="150" as="geometry"/>
</mxCell>
```

### Frame Colors by Purpose

| Purpose | Fill Color | Stroke Color |
|---------|------------|--------------|
| Public/External | `#d5e8d4` (light green) | `#82b366` |
| Private/Internal | `#dae8fc` (light blue) | `#6c8ebf` |
| Security Boundary | `#f8cecc` (light red) | `#b85450` |
| Optional/Future | `none` | `#999999` (dashed) |
| Highlight/Important | `#fff2cc` (light yellow) | `#d6b656` |

---

## Waypoints and Edge Control Points

Control edge routing with waypoints for precise path control.

### Adding Waypoints

```xml
<mxCell id="edge-1" style="edgeStyle=orthogonalEdgeStyle;rounded=1;endArrow=classic;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="200" y="100"/>
      <mxPoint x="200" y="200"/>
      <mxPoint x="300" y="200"/>
    </Array>
  </mxGeometry>
</mxCell>
```

### Entry and Exit Points

Control where edges connect to shapes:

```xml
<mxCell id="edge-1" style="edgeStyle=orthogonalEdgeStyle;entryX=0;entryY=0.5;exitX=1;exitY=0.5;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

| Attribute | Description |
|-----------|-------------|
| `entryX` | Target connection X (0=left, 0.5=center, 1=right) |
| `entryY` | Target connection Y (0=top, 0.5=middle, 1=bottom) |
| `exitX` | Source connection X |
| `exitY` | Source connection Y |
| `entryDx` | Target X offset in pixels |
| `entryDy` | Target Y offset in pixels |
| `exitDx` | Source X offset in pixels |
| `exitDy` | Source Y offset in pixels |

---

## Example: AWS ECS Architecture Cell

```xml
<mxCell id="ecs-1" value="ECS Cluster"
  style="sketch=0;outlineConnect=0;fontColor=#232F3E;gradientColor=none;strokeColor=#ffffff;fillColor=#ED7100;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;align=center;html=1;fontSize=12;fontStyle=0;aspect=fixed;shape=mxgraph.aws4.ecs;labelBackgroundColor=none;"
  vertex="1" parent="1">
  <mxGeometry x="200" y="150" width="78" height="78" as="geometry"/>
</mxCell>
```

## Example: Complete AWS Architecture Snippet (with proper nesting)

```xml
<mxfile host="app.diagrams.net" modified="2024-01-01T00:00:00.000Z" agent="Claude">
  <diagram name="AWS-Architecture" id="aws-arch-1">
    <mxGraphModel dx="1200" dy="800" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="850" pageHeight="1100">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>

        <!-- VPC Group (top-level, parent="1") -->
        <mxCell id="vpc-group" value="VPC" style="sketch=0;outlineConnect=0;gradientColor=none;html=1;whiteSpace=wrap;fontSize=12;fontStyle=0;shape=mxgraph.aws4.group;grIcon=mxgraph.aws4.group_vpc;strokeColor=#248814;fillColor=none;verticalAlign=top;align=left;spacingLeft=30;fontColor=#248814;dashed=0;" vertex="1" parent="1">
          <mxGeometry x="40" y="40" width="400" height="300" as="geometry"/>
        </mxCell>

        <!-- ECS Service (INSIDE VPC: parent="vpc-group", coords relative to VPC) -->
        <mxCell id="ecs-1" value="ECS" style="sketch=0;outlineConnect=0;fontColor=#232F3E;gradientColor=none;fillColor=#ED7100;strokeColor=#ffffff;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;align=center;html=1;fontSize=12;fontStyle=0;aspect=fixed;shape=mxgraph.aws4.ecs;" vertex="1" parent="vpc-group">
          <mxGeometry x="40" y="80" width="60" height="60" as="geometry"/>
        </mxCell>

        <!-- RDS Database (INSIDE VPC: parent="vpc-group", coords relative to VPC) -->
        <mxCell id="rds-1" value="RDS" style="sketch=0;outlineConnect=0;fontColor=#232F3E;gradientColor=none;fillColor=#C925D1;strokeColor=#ffffff;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;align=center;html=1;fontSize=12;fontStyle=0;aspect=fixed;shape=mxgraph.aws4.rds;" vertex="1" parent="vpc-group">
          <mxGeometry x="200" y="80" width="60" height="60" as="geometry"/>
        </mxCell>

        <!-- Connection Arrow (parent="1" - edges stay at root level) -->
        <mxCell id="edge-1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#232F3E;strokeWidth=2;" edge="1" parent="1" source="ecs-1" target="rds-1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

**Key points in this example:**
- `vpc-group` has `parent="1"` (top-level container)
- `ecs-1` and `rds-1` have `parent="vpc-group"` (nested inside VPC)
- Coordinates for ECS/RDS are relative to VPC's top-left corner
- Edge `parent="1"` keeps connections at root level for proper rendering

---

## Style Guidelines

### Common Style Attributes
- `sketch=0` - Clean lines (not hand-drawn)
- `outlineConnect=0` - Connection points at shape edge
- `aspect=fixed` - Maintain aspect ratio
- `verticalLabelPosition=bottom` - Label below icon
- `verticalAlign=top` - Align icon to top
- `html=1` - Enable HTML in labels
- `shadow=1` - **REQUIRED**: All shapes, text labels, and edges must have drop shadow
- `textShadow=1` - **REQUIRED**: All elements with text labels must have text shadow

### Shadow (Required for All Elements)

**All service icons, text labels, and edges must have shadow enabled** for visual depth and clarity.

**Two shadow attributes:**
- `shadow=1` - Drop shadow on the element (shape, edge, text box)
- `textShadow=1` - Shadow on text/labels within the element

```xml
<!-- Shape with shadow on icon and text label -->
<mxCell id="ecs-1" value="ECS"
  style="...;shadow=1;textShadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="60" height="60" as="geometry"/>
</mxCell>

<!-- Text label with shadow -->
<mxCell id="label-1" value="Security Group Rules"
  style="text;html=1;strokeColor=#7AA116;fillColor=#d5e8d4;align=left;verticalAlign=top;whiteSpace=wrap;rounded=1;fontSize=8;fontColor=#333333;spacing=4;shadow=1;textShadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="60" y="350" width="140" height="50" as="geometry"/>
</mxCell>

<!-- Edge with shadow on line and text label -->
<mxCell id="edge-1" value="HTTPS:443"
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=classic;endFill=1;flowAnimation=1;shadow=1;textShadow=1;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### Stroke Width Hierarchy

**Groups/Frames use hierarchical stroke widths based on nesting level:**

| Level | Element Type | Stroke Width |
|-------|--------------|--------------|
| 1 (Top) | AWS Cloud, Azure Subscription | `strokeWidth=4` |
| 2 | Region, Resource Group | `strokeWidth=3` |
| 3 | VPC, Virtual Network | `strokeWidth=3` |
| 4+ | Subnets, Security Groups | `strokeWidth=2` |

**Edges/Arrows always use consistent width:**

| Edge Type | Stroke Width |
|-----------|--------------|
| All arrows/connections | `strokeWidth=2` |

```xml
<!-- Top-level group (4pt) -->
<mxCell id="aws-cloud" value="AWS Cloud"
  style="...;strokeWidth=4;" vertex="1" parent="1">

<!-- Sub-group (3pt) -->
<mxCell id="region" value="Region"
  style="...;strokeWidth=3;" vertex="1" parent="aws-cloud">

<!-- Subnet (2pt) -->
<mxCell id="subnet" value="Public Subnet"
  style="...;strokeWidth=2;" vertex="1" parent="vpc">

<!-- Edge (always 2pt) -->
<mxCell id="edge-1" style="...;strokeWidth=2;" edge="1" parent="1">
```

### AWS Color Palette
| Category | Fill Color |
|----------|------------|
| Compute (Orange) | `#ED7100` |
| Storage (Green) | `#7AA116` |
| Database (Purple) | `#C925D1` |
| Networking (Purple) | `#8C4FFF` |
| Security (Red) | `#DD344C` |
| Analytics (Blue) | `#006FB7` |

### Recommended Icon Size
- Standard icons: `60x60` or `78x78`
- Group containers: Scale based on content
- Minimum spacing between icons: `40px`

---

## Flow Animation

**Unidirectional arrows (single arrow head) should have flow animation** to indicate data direction.

### Flow Animation Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| `flowAnimation` | `1` | Enable animated flow along edge |

### Flow Animation Example

```xml
<!-- Animated flow arrow -->
<mxCell id="edge-flow" value="HTTPS"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#232F3E;strokeWidth=2;endArrow=classic;endFill=1;flowAnimation=1;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

**When to use flow animation:**
- Data flow connections (API calls, requests)
- Network traffic paths
- Message/event flows

**When NOT to use flow animation:**
- Bidirectional arrows
- Management/control relationships (dashed lines)
- Static associations

---

## Edge Routing Best Practices

### CRITICAL: Avoid Crossing Shapes

**Edges must NEVER cross over shapes.** Use waypoints to route around obstacles.

### Routing Strategies

1. **Use waypoints to route around shapes:**
```xml
<mxCell id="edge-1" style="edgeStyle=orthogonalEdgeStyle;rounded=1;endArrow=classic;"
  edge="1" parent="1" source="a" target="b">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="150" y="50"/>  <!-- Route above obstacle -->
      <mxPoint x="300" y="50"/>
    </Array>
  </mxGeometry>
</mxCell>
```

2. **Use entry/exit points to control connection angles:**
```xml
<!-- Connect from right side of source to left side of target -->
<mxCell style="exitX=1;exitY=0.5;entryX=0;entryY=0.5;" ...>
```

3. **Route edges along container boundaries** rather than through them

### Edge Spacing

- Maintain minimum `20px` gap between parallel edges
- Keep edges at least `10px` away from shape boundaries
- Align waypoints to grid (10px increments) for clean routing

---

## Instructions for Claude

### Shape and Structure
1. **Always use official shape references** - Never use generic rectangles for cloud services
2. **Match provider to request** - If user says "AWS diagram", use `mxgraph.aws4.*` shapes
3. **Include proper groups** - Use VPC/Subnet/Region groups to show architecture boundaries
4. **CRITICAL: Set correct parent attributes** - Children must have `parent="<container-id>"` to move with their container
5. **Use relative coordinates for nested elements** - Child coordinates are relative to parent's top-left
6. **XML order determines z-order** - Containers first → Children after → Edges last (inner items render on top)
7. **REQUIRED: Add shadow to all elements** - Every service icon, text label, and edge must have `shadow=1;textShadow=1`

### Lines and Arrows
8. **Keep edges at root level** - Connection edges should have `parent="1"`
9. **Choose edge style by layout density:**
    - `orthogonalEdgeStyle` - Default for clean diagrams with spaced shapes
    - `none` (straight) - Dense layouts where orthogonal routing causes crossings
10. **All edges use strokeWidth=2** - Consistent 2pt stroke for all arrows and connections
11. **Add flow animation to unidirectional arrows** - Set `flowAnimation=1` for single-head arrows showing data flow
12. **Choose arrow type by connection semantics:**
    - `classic` - HTTP requests, data flow, publish operations
    - `open` - Lightweight connections (runs-on, metrics, telemetry)
    - `async` - Async messaging, polling, event consumption
    - `block` - Pull/fetch operations (image pull, data fetch)
    - Bidirectional (`startArrow` + `endArrow`) - Read/write (database, cache)
    - `none` - Associations without direction
13. **Add edge labels for protocols** - Label connections with protocol names (HTTPS, gRPC, etc.)
14. **Use dashed lines for management relationships** - Set `dashed=1` for control plane, monitoring, or optional paths

### Frames and Containers
15. **Use hierarchical stroke widths for groups:**
    - Top-level (AWS Cloud): `strokeWidth=4`
    - Sub-groups (Region, VPC): `strokeWidth=3`
    - Leaf groups (Subnets): `strokeWidth=2`
16. **Group shapes must have square corners** - Always use `rounded=0` for container/group shapes
17. **Apply consistent frame colors** - Green for public, blue for private, red for security boundaries
18. **Position frame labels consistently** - Use `verticalAlign=top;align=left` for frame titles

### Edge Routing and Shape Positioning
19. **CRITICAL: Edges must NEVER cross shapes** - Position shapes so connected items are adjacent
20. **Position connected shapes adjacently** - Shapes that connect should be neighbors (horizontally or vertically)
21. **Use row-based layout** - Organize shapes in rows where each row represents a logical tier
22. **Stagger non-connected shapes** - Avoid grid alignment that forces diagonal crossings
23. **Use entry/exit points** - Control connection angles with `exitX`, `exitY`, `entryX`, `entryY`
24. **Maintain edge spacing** - Keep 20px between parallel edges, 10px from shapes

### General Guidelines
25. **Use correct colors** - Follow the provider's color palette for each service category
26. **Label all components** - Include service names in the `value` attribute
27. **Position logically** - Arrange components following left-to-right or top-to-bottom flow
