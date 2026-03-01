# CLAUDE.md - DrawIO Architecture Diagram Guidelines

## Overview

Comprehensive guide for creating `.drawio` architecture diagrams covering:
- **Cloud Platforms**: AWS, Azure, Google Cloud (GCP)
- **Container Orchestration**: Kubernetes (K8s), Docker
- **Network & Infrastructure**: Firewalls, Load Balancers, DNS, VPN
- **Software Architecture**: Microservices, APIs, Databases
- **C4 Model**: Context, Container, Component diagrams for software architecture
- **Integration Systems**: Message Queues, ESB, API Gateways
- **IT Development**: CI/CD, DevOps, Monitoring
- **Database ERD**: Entity-Relationship Diagrams with multi-database support (SQL, NoSQL, Cache, Search)

DrawIO uses XML format with specific style attributes to reference official shape libraries for each domain.

### Important: Skip Irrelevant Diagram Types

> **Note:** Not all diagram types apply to every project. If the target project does not have sufficient information for a specific diagram type, **skip creating that diagram entirely**.
>
> **Examples:**
> - No database/ORM → Skip ERD diagrams
> - No cloud infrastructure → Skip AWS/Azure/GCP diagrams
> - No Kubernetes → Skip K8s diagrams
> - No microservices → Skip complex service mesh diagrams
> - No message queues → Skip async messaging flow diagrams
> - Simple monolith → Skip C4 Component level diagrams
> - No CI/CD pipeline → Skip DevOps diagrams
>
> **Do not create empty or placeholder diagrams.** Only generate diagrams for aspects that actually exist in the codebase or infrastructure.
>
> **IMPORTANT: Architecture Overview Diagram is ALWAYS required.** The `architecture-overview.drawio` file serves as the main visual representation of the project structure. It must be created for every project regardless of which other diagram types apply. This diagram should show:
> - Overall system/project structure
> - Main components/modules/packages
> - Key relationships and dependencies
> - Technology stack visualization

---

## Table of Contents

### Required Diagrams
- **Architecture Overview (`architecture-overview.drawio`)** - ALWAYS REQUIRED for every project

### Fundamentals
- [File Structure](#file-structure)
- [CRITICAL: Parent-Child Relationships for Grouping](#critical-parent-child-relationships-for-grouping)

### Cloud Provider Shape References
- [AWS Shape References](#aws-shape-references) - Compute, Storage, Database, Networking, Security, Messaging
- [Azure Shape References](#azure-shape-references) - Compute, Storage, Database, Networking, Integration
- [Google Cloud Platform (GCP) Shape References](#google-cloud-platform-gcp-shape-references)
- [Kubernetes Shape References](#kubernetes-shape-references) - Workloads, Networking, Config, Cluster

### Infrastructure & Architecture
- [Network & Infrastructure Shape References](#network--infrastructure-shape-references) - General, Cisco, On-Premises
- [C4 Model Architecture Diagrams](#c4-model-architecture-diagrams) - Context, Container, Component levels
- [Software Architecture Shape References](#software-architecture-shape-references) - UML, Microservices, APIs
- [DevOps & CI/CD Shape References](#devops--cicd-shape-references)

### Database & Data
- [Database Shape References](#database-shape-references) - SQL, NoSQL, Cloud-Managed, Data Warehouse
- [Search Engine & Analytics Shape References](#search-engine--analytics-shape-references)
- [Data Workflow & ETL Shape References](#data-workflow--etl-shape-references) - Pipeline, Stream Processing
- [Workflow & Process Diagram Shape References](#workflow--process-diagram-shape-references) - BPMN, Flowchart, State Machine
- [Database ERD (Entity-Relationship Diagrams)](#database-erd-entity-relationship-diagrams) - Table structure, Relationships
- [NoSQL / DynamoDB Data Model Diagrams](#nosql--dynamodb-data-model-diagrams) - Partition keys, Sort keys, TTL
- [Conceptual ERD for NoSQL](#conceptual-erd-for-nosql-domain-based) - Domain-based grouping
- [Project Structure Diagrams](#project-structure-diagrams) - Folder organization, Module hierarchy

### Lines, Arrows & Styling
- [Lines and Edges (Connections)](#lines-and-edges-connections) - Edge styles, routing
- [Arrow Color Standards (Protocol-Based)](#arrow-color-standards-protocol-based) - HTTP, Database, Cache, Queue colors
- [CRITICAL: Mandatory Arrow Legend Requirements](#critical-mandatory-arrow-legend-requirements) - **Required legends for meaningful arrows**
- [Arrows (Edge Endpoints)](#arrows-edge-endpoints) - Arrow types, selection guide
- [Frames and Containers](#frames-and-containers) - Grouping, styling
- [Waypoints and Edge Control Points](#waypoints-and-edge-control-points)

### Examples & Guidelines
- [Example: AWS ECS Architecture Cell](#example-aws-ecs-architecture-cell)
- [Example: Complete AWS Architecture Snippet](#example-complete-aws-architecture-snippet-with-proper-nesting)
- [Style Guidelines](#style-guidelines) - Shadows, stroke widths, colors
- [Flow Animation](#flow-animation)
- [Edge Routing Best Practices](#edge-routing-best-practices)
- [Instructions for Claude](#instructions-for-claude) - Shape, Lines, Frames, Domain-specific

---

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

### Default Group/Container Style (REQUIRED)

**All group containers MUST use the swimlane style with the following mandatory attributes:**

```xml
<mxCell id="container-id" value="Container Name"
  style="swimlane;startSize=0;strokeWidth=2;labelBorderColor=none;textShadow=1;labelBackgroundColor=default;fillColor=#F5F5F5;strokeColor=#666666;fontSize=14;fontStyle=1;shadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="40" y="80" width="400" height="300" as="geometry"/>
</mxCell>
```

**Mandatory Style Attributes:**
| Attribute | Value | Description |
|-----------|-------|-------------|
| `startSize=0` | `0` | No header area - label floats on top of container |
| `labelBorderColor=none` | `none` | Removes border around label text |
| `textShadow=1` | `1` | Adds shadow behind label text for readability |
| `labelBackgroundColor=default` | `default` | White background behind label text |

**Why These Attributes:**
- `startSize=0` - Allows content to use full container space without a reserved header area
- `labelBorderColor=none` + `textShadow=1` + `labelBackgroundColor=default` - Creates a clean, readable label that floats over the container background with good contrast

**Example with Children (proper parent-child relationship):**
```xml
<!-- Container -->
<mxCell id="zone-users" value="Users Zone"
  style="swimlane;startSize=0;strokeWidth=2;labelBorderColor=none;textShadow=1;labelBackgroundColor=default;fillColor=#F5F5F5;strokeColor=#666666;fontSize=14;fontStyle=1;dashed=1;shadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="40" y="80" width="200" height="120" as="geometry"/>
</mxCell>

<!-- Child with relative coordinates (relative to container's top-left corner) -->
<mxCell id="user-finance" value="Finance User"
  style="shape=umlActor;fillColor=#08427B;strokeColor=#052E56;fontColor=#FFFFFF;"
  parent="zone-users" vertex="1">
  <mxGeometry x="70" y="25" width="60" height="60" as="geometry"/>
</mxCell>
```

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

**Visual Z-Order (1=front, 4=back):**
```
1. Standalone shapes (parent="1")  (front)
2. Text Labels
3. Edges
4. Groups + nested children       (back)
```

**XML Definition Order:**
```
1. Groups with children immediately after each group
2. Edges
3. Text Labels
4. Standalone shapes (parent="1") ← last in XML → front
```

**Important:** Shapes nested inside groups (`parent="group-id"`) are constrained to that group's z-layer. Only root-level shapes (`parent="1"`) can be reordered freely relative to edges.

**Example XML Structure:**
```xml
<!-- Groups with children defined immediately after -->
<mxCell id="vpc" value="VPC" ... />
  <mxCell id="subnet" parent="vpc" ... />
    <mxCell id="ecs" parent="subnet" ... />  <!-- child of subnet -->
    <mxCell id="rds" parent="subnet" ... />  <!-- child of subnet -->

<!-- Edges -->
<mxCell id="edge-1" parent="1" source="ecs" target="rds" edge="1" />

<!-- Text Labels -->
<mxCell id="label-1" value="Security Rules" ... />

<!-- Standalone shapes (front) -->
<mxCell id="users" parent="1" ... />  <!-- root-level, renders on top -->
```

**Rules:**
- **Edges must have `parent="1"`** - They connect across containers and need to be at root level
- **Shapes use relative coordinates** - Child coordinates are relative to parent's top-left
- **Container must have `vertex="1"`** - Required to be a valid parent for nesting

---

## AWS Shape References

AWS service icons use the `resourceIcon` shape with `resIcon` parameter for AWS Architecture 2024 icons.

### AWS Icon Style Format

```xml
<mxCell id="sqs" value="SQS"
  style="sketch=0;points=[[0,0,0],[0.25,0,0],[0.5,0,0],[0.75,0,0],[1,0,0],[0,1,0],[0.25,1,0],[0.5,1,0],[0.75,1,0],[1,1,0],[0,0.25,0],[0,0.5,0],[0,0.75,0],[1,0.25,0],[1,0.5,0],[1,0.75,0]];outlineConnect=0;fontColor=#232F3E;fillColor=#E7157B;strokeColor=#ffffff;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;align=center;html=1;fontSize=11;fontStyle=0;aspect=fixed;shape=mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.sqs;shadow=1;textShadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="60" height="60" as="geometry"/>
</mxCell>
```

### Compute
| Service | resIcon Value | Fill Color |
|---------|---------------|------------|
| EC2 | `resIcon=mxgraph.aws4.ec2` | `#ED7100` |
| Lambda | `resIcon=mxgraph.aws4.lambda_function` | `#ED7100` |
| ECS | `resIcon=mxgraph.aws4.ecs` | `#ED7100` |
| ECS Service | `resIcon=mxgraph.aws4.ecs_service` | `#ED7100` |
| EKS | `resIcon=mxgraph.aws4.eks` | `#ED7100` |
| Fargate | `resIcon=mxgraph.aws4.fargate` | `#ED7100` |
| ECR | `resIcon=mxgraph.aws4.ecr` | `#ED7100` |
| Elastic Beanstalk | `resIcon=mxgraph.aws4.elastic_beanstalk` | `#ED7100` |

### Storage
| Service | resIcon Value | Fill Color |
|---------|---------------|------------|
| S3 | `resIcon=mxgraph.aws4.s3` | `#7AA116` |
| EBS | `resIcon=mxgraph.aws4.elastic_block_store` | `#7AA116` |
| EFS | `resIcon=mxgraph.aws4.elastic_file_system` | `#7AA116` |
| Glacier | `resIcon=mxgraph.aws4.glacier` | `#7AA116` |

### Database
| Service | resIcon Value | Fill Color |
|---------|---------------|------------|
| RDS | `resIcon=mxgraph.aws4.rds` | `#C925D1` |
| DynamoDB | `resIcon=mxgraph.aws4.dynamodb` | `#C925D1` |
| Aurora | `resIcon=mxgraph.aws4.aurora` | `#C925D1` |
| ElastiCache | `resIcon=mxgraph.aws4.elasticache` | `#C925D1` |
| Redshift | `resIcon=mxgraph.aws4.redshift` | `#C925D1` |

### Networking
| Service | resIcon Value | Fill Color |
|---------|---------------|------------|
| VPC | `resIcon=mxgraph.aws4.vpc` | `#8C4FFF` |
| CloudFront | `resIcon=mxgraph.aws4.cloudfront` | `#8C4FFF` |
| Route 53 | `resIcon=mxgraph.aws4.route_53` | `#8C4FFF` |
| API Gateway | `resIcon=mxgraph.aws4.api_gateway` | `#8C4FFF` |
| ALB | `resIcon=mxgraph.aws4.application_load_balancer` | `#8C4FFF` |
| NLB | `resIcon=mxgraph.aws4.network_load_balancer` | `#8C4FFF` |
| NAT Gateway | `resIcon=mxgraph.aws4.nat_gateway` | `#8C4FFF` |
| Internet Gateway | `resIcon=mxgraph.aws4.internet_gateway` | `#8C4FFF` |

### Security
| Service | resIcon Value | Fill Color |
|---------|---------------|------------|
| IAM | `resIcon=mxgraph.aws4.iam` | `#DD344C` |
| Cognito | `resIcon=mxgraph.aws4.cognito` | `#DD344C` |
| KMS | `resIcon=mxgraph.aws4.key_management_service` | `#DD344C` |
| WAF | `resIcon=mxgraph.aws4.waf` | `#DD344C` |
| Secrets Manager | `resIcon=mxgraph.aws4.secrets_manager` | `#DD344C` |

### Messaging & Integration
| Service | resIcon Value | Fill Color |
|---------|---------------|------------|
| SQS | `resIcon=mxgraph.aws4.sqs` | `#E7157B` |
| SNS | `resIcon=mxgraph.aws4.sns` | `#E7157B` |
| EventBridge | `resIcon=mxgraph.aws4.eventbridge` | `#E7157B` |
| Step Functions | `resIcon=mxgraph.aws4.step_functions` | `#E7157B` |

### Monitoring
| Service | resIcon Value | Fill Color |
|---------|---------------|------------|
| CloudWatch | `resIcon=mxgraph.aws4.cloudwatch` | `#E7157B` |
| X-Ray | `resIcon=mxgraph.aws4.xray` | `#E7157B` |

### General/Users
| Service | resIcon Value | Fill Color |
|---------|---------------|------------|
| Users | `resIcon=mxgraph.aws4.users` | `#232F3E` |
| Client | `resIcon=mxgraph.aws4.client` | `#232F3E` |

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

Azure uses **image-based SVG icons** from the `azure2` library. Use the `image=img/lib/azure2/category/Icon_Name.svg` format.

### Azure Icon Style Format

```xml
<mxCell id="app-service" value="App Service"
  style="aspect=fixed;html=1;points=[];align=center;image;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;fontSize=10;imageAspect=0;image=img/lib/azure2/app_services/App_Services.svg;shadow=1;textShadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="60" height="60" as="geometry"/>
</mxCell>
```

### Compute
| Service | Image Path |
|---------|------------|
| Virtual Machine | `image=img/lib/azure2/compute/Virtual_Machine.svg` |
| App Service | `image=img/lib/azure2/app_services/App_Services.svg` |
| Functions | `image=img/lib/azure2/compute/Function_Apps.svg` |
| AKS | `image=img/lib/azure2/containers/Kubernetes_Services.svg` |
| Container Instances | `image=img/lib/azure2/compute/Container_Instances.svg` |
| Container Registry | `image=img/lib/azure2/containers/Container_Registries.svg` |

### Storage
| Service | Image Path |
|---------|------------|
| Storage Account | `image=img/lib/azure2/storage/Storage_Accounts.svg` |
| Blob Storage | `image=img/lib/azure2/general/Blob_Block.svg` |
| Data Lake | `image=img/lib/azure2/storage/Data_Lake_Storage_Gen1.svg` |
| Azure Files | `image=img/lib/azure2/storage/Azure_Fileshare.svg` |

### Database
| Service | Image Path |
|---------|------------|
| SQL Database | `image=img/lib/azure2/databases/Azure_SQL.svg` |
| Cosmos DB | `image=img/lib/azure2/databases/Azure_Cosmos_DB.svg` |
| Redis Cache | `image=img/lib/azure2/databases/Cache_Redis.svg` |
| MySQL | `image=img/lib/azure2/databases/Azure_Database_MySQL_Server.svg` |
| PostgreSQL | `image=img/lib/azure2/databases/Azure_Database_PostgreSQL_Server.svg` |

### Networking
| Service | Image Path |
|---------|------------|
| Virtual Network | Use rectangle container with `strokeColor=#0078D4` |
| Load Balancer | `image=img/lib/azure2/networking/Load_Balancers.svg` |
| Application Gateway | `image=img/lib/azure2/networking/Application_Gateways.svg` |
| CDN | `image=img/lib/azure2/networking/CDN_Profiles.svg` |
| DNS Zone | `image=img/lib/azure2/networking/DNS_Zones.svg` |
| Firewall | `image=img/lib/azure2/networking/Firewalls.svg` |
| Front Door | `image=img/lib/azure2/networking/Front_Doors.svg` |

### Security & Identity
| Service | Image Path |
|---------|------------|
| Key Vault | `image=img/lib/azure2/security/Key_Vaults.svg` |
| Azure AD | `image=img/lib/azure2/identity/Azure_Active_Directory.svg` |
| Users | `image=img/lib/azure2/identity/Users.svg` |
| Managed Identity | `image=img/lib/azure2/identity/Managed_Identities.svg` |

### Integration
| Service | Image Path |
|---------|------------|
| Service Bus | `image=img/lib/azure2/integration/Service_Bus.svg` |
| Logic Apps | `image=img/lib/azure2/integration/Logic_Apps.svg` |
| Event Grid | `image=img/lib/azure2/integration/Event_Grid_Topics.svg` |
| API Management | `image=img/lib/azure2/integration/API_Management_Services.svg` |

### Monitoring
| Service | Image Path |
|---------|------------|
| Application Insights | `image=img/lib/azure2/devops/Application_Insights.svg` |
| Log Analytics | `image=img/lib/azure2/management_governance/Log_Analytics_Workspaces.svg` |

### Azure Container Groups (Subscription, Resource Group, VNet)

Azure containers use simple rectangle shapes with Azure blue color (`#0078D4`):

```xml
<!-- Azure Subscription (Top Level - strokeWidth=4) -->
<mxCell id="azure-sub" value="Azure Subscription"
  style="rounded=0;whiteSpace=wrap;html=1;strokeColor=#0078D4;fillColor=none;verticalAlign=top;align=left;spacingLeft=10;fontColor=#0078D4;dashed=0;strokeWidth=4;fontSize=14;fontStyle=1;shadow=1;textShadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="120" y="40" width="1160" height="720" as="geometry"/>
</mxCell>

<!-- Resource Group (Level 2 - strokeWidth=3, dashed) -->
<mxCell id="rg-webapp" value="rg-webapp-prod"
  style="rounded=0;whiteSpace=wrap;html=1;strokeColor=#0078D4;fillColor=none;verticalAlign=top;align=left;spacingLeft=10;fontColor=#0078D4;dashed=1;strokeWidth=3;fontSize=12;shadow=1;textShadow=1;"
  vertex="1" parent="azure-sub">
  <mxGeometry x="20" y="40" width="1120" height="660" as="geometry"/>
</mxCell>

<!-- Virtual Network (Level 3 - strokeWidth=3, light blue fill) -->
<mxCell id="vnet" value="vnet-webapp (10.0.0.0/16)"
  style="rounded=0;whiteSpace=wrap;html=1;strokeColor=#0078D4;fillColor=#E6F2FF;verticalAlign=top;align=left;spacingLeft=10;fontColor=#0078D4;dashed=0;strokeWidth=3;fontSize=11;shadow=1;textShadow=1;"
  vertex="1" parent="rg-webapp">
  <mxGeometry x="200" y="40" width="900" height="600" as="geometry"/>
</mxCell>
```

### Azure Subnet Colors

| Subnet Type | Fill Color | Stroke Color |
|-------------|------------|--------------|
| Public | `#d5e8d4` | `#82b366` |
| App/Private | `#dae8fc` | `#6c8ebf` |
| Data | `#e1d5e7` | `#9673a6` |
| Integration | `#fff2cc` | `#d6b656` |

---

## Kubernetes Shape References

Use `shape=mxgraph.kubernetes.` prefix for Kubernetes icons.

### Workloads
| Resource | Shape Style |
|----------|-------------|
| Pod | `shape=mxgraph.kubernetes.pod` |
| Deployment | `shape=mxgraph.kubernetes.deploy` |
| StatefulSet | `shape=mxgraph.kubernetes.sts` |
| DaemonSet | `shape=mxgraph.kubernetes.ds` |
| ReplicaSet | `shape=mxgraph.kubernetes.rs` |
| Job | `shape=mxgraph.kubernetes.job` |
| CronJob | `shape=mxgraph.kubernetes.cronjob` |

### Networking
| Resource | Shape Style |
|----------|-------------|
| Service | `shape=mxgraph.kubernetes.svc` |
| Ingress | `shape=mxgraph.kubernetes.ing` |
| Endpoint | `shape=mxgraph.kubernetes.ep` |
| Network Policy | `shape=mxgraph.kubernetes.netpol` |

### Config & Storage
| Resource | Shape Style |
|----------|-------------|
| ConfigMap | `shape=mxgraph.kubernetes.cm` |
| Secret | `shape=mxgraph.kubernetes.secret` |
| PersistentVolume | `shape=mxgraph.kubernetes.pv` |
| PersistentVolumeClaim | `shape=mxgraph.kubernetes.pvc` |
| StorageClass | `shape=mxgraph.kubernetes.sc` |

### Cluster
| Resource | Shape Style |
|----------|-------------|
| Namespace | `shape=mxgraph.kubernetes.ns` |
| Node | `shape=mxgraph.kubernetes.node` |
| Cluster | `shape=mxgraph.kubernetes.control_plane` |
| ServiceAccount | `shape=mxgraph.kubernetes.sa` |
| Role | `shape=mxgraph.kubernetes.role` |
| ClusterRole | `shape=mxgraph.kubernetes.c_role` |

---

## Google Cloud Platform (GCP) Shape References

Use `shape=mxgraph.gcp2.` prefix for GCP icons.

### Compute
| Service | Shape Style |
|---------|-------------|
| Compute Engine | `shape=mxgraph.gcp2.compute_engine` |
| App Engine | `shape=mxgraph.gcp2.app_engine` |
| Cloud Functions | `shape=mxgraph.gcp2.cloud_functions` |
| Cloud Run | `shape=mxgraph.gcp2.cloud_run` |
| GKE | `shape=mxgraph.gcp2.google_kubernetes_engine` |

### Storage & Database
| Service | Shape Style |
|---------|-------------|
| Cloud Storage | `shape=mxgraph.gcp2.cloud_storage` |
| Cloud SQL | `shape=mxgraph.gcp2.cloud_sql` |
| Cloud Spanner | `shape=mxgraph.gcp2.cloud_spanner` |
| Firestore | `shape=mxgraph.gcp2.cloud_firestore` |
| Bigtable | `shape=mxgraph.gcp2.cloud_bigtable` |
| Memorystore | `shape=mxgraph.gcp2.cloud_memorystore` |

### Networking
| Service | Shape Style |
|---------|-------------|
| VPC | `shape=mxgraph.gcp2.virtual_private_cloud` |
| Cloud Load Balancing | `shape=mxgraph.gcp2.cloud_load_balancing` |
| Cloud CDN | `shape=mxgraph.gcp2.cloud_cdn` |
| Cloud DNS | `shape=mxgraph.gcp2.cloud_dns` |
| Cloud VPN | `shape=mxgraph.gcp2.cloud_vpn` |
| Cloud Armor | `shape=mxgraph.gcp2.cloud_armor` |

### Integration & Messaging
| Service | Shape Style |
|---------|-------------|
| Pub/Sub | `shape=mxgraph.gcp2.cloud_pubsub` |
| Cloud Tasks | `shape=mxgraph.gcp2.cloud_tasks` |
| Cloud Scheduler | `shape=mxgraph.gcp2.cloud_scheduler` |
| Workflows | `shape=mxgraph.gcp2.workflows` |

### Security & Identity
| Service | Shape Style |
|---------|-------------|
| IAM | `shape=mxgraph.gcp2.cloud_iam` |
| Secret Manager | `shape=mxgraph.gcp2.secret_manager` |
| Cloud KMS | `shape=mxgraph.gcp2.cloud_key_management_service` |

### DevOps & Monitoring
| Service | Shape Style |
|---------|-------------|
| Cloud Build | `shape=mxgraph.gcp2.cloud_build` |
| Container Registry | `shape=mxgraph.gcp2.container_registry` |
| Artifact Registry | `shape=mxgraph.gcp2.artifact_registry` |
| Cloud Monitoring | `shape=mxgraph.gcp2.cloud_monitoring` |
| Cloud Logging | `shape=mxgraph.gcp2.cloud_logging` |

---

## Network & Infrastructure Shape References

### General Network Shapes
Use `shape=mxgraph.networks.` or basic shapes for network diagrams.

| Component | Shape Style |
|-----------|-------------|
| Router | `shape=mxgraph.networks.router` |
| Switch | `shape=mxgraph.networks.switch` |
| Firewall | `shape=mxgraph.networks.firewall` |
| Load Balancer | `shape=mxgraph.networks.load_balancer` |
| Server | `shape=mxgraph.networks.server` |
| Database Server | `shape=mxgraph.networks.server_database` |
| Web Server | `shape=mxgraph.networks.server_web` |
| Cloud | `shape=mxgraph.networks.cloud` |
| User/Client | `shape=mxgraph.networks.user_male` |
| Laptop | `shape=mxgraph.networks.laptop` |
| Desktop | `shape=mxgraph.networks.desktop` |
| Mobile | `shape=mxgraph.networks.mobile` |

### Cisco Network Icons
Use `shape=mxgraph.cisco.` for Cisco-style network diagrams.

| Component | Shape Style |
|-----------|-------------|
| Router | `shape=mxgraph.cisco.routers.router` |
| Switch | `shape=mxgraph.cisco.switches.layer_3_switch` |
| Firewall | `shape=mxgraph.cisco.security.firewall` |
| VPN | `shape=mxgraph.cisco.security.vpn_concentrator` |
| Wireless | `shape=mxgraph.cisco.wireless.access_point` |
| Server | `shape=mxgraph.cisco.servers.standard_host` |

### On-Premises / Data Center
| Component | Shape Style |
|-----------|-------------|
| Data Center | `shape=mxgraph.aws4.group;grIcon=mxgraph.aws4.group_corporate_data_center` |
| Rack | `shape=mxgraph.rackGeneral.container` |
| Physical Server | `shape=mxgraph.veeam.physical_server` |
| Virtual Machine | `shape=mxgraph.veeam.virtual_machine` |
| Storage Array | `shape=mxgraph.veeam.storage` |
| Tape Library | `shape=mxgraph.veeam.tape` |

---

## C4 Model Architecture Diagrams

C4 Model provides a hierarchical approach to software architecture visualization with four levels: Context, Container, Component, and Code.

### C4 Diagram Levels

| Level | Purpose | Scope |
|-------|---------|-------|
| **Context** | System landscape, users, external systems | Highest level - who uses the system |
| **Container** | Applications, databases, services within the system | Technology choices, deployment units |
| **Component** | Internal structure of a container | Classes, modules, services |
| **Code** | Implementation details (rarely used in DrawIO) | UML class diagrams |

### C4 Color Scheme

| Element Type | Fill Color | Stroke Color | Font Color |
|--------------|------------|--------------|------------|
| Person (Actor) | `#08427B` | `#052E56` | `#FFFFFF` |
| Software System (Internal) | `#438DD5` | `#3C7FC0` | `#FFFFFF` |
| External System | `#999999` | `#8A8A8A` | `#FFFFFF` |
| Container | `#438DD5` | `#3C7FC0` | `#FFFFFF` |
| Component | `#85BBF0` | `#5A9BD5` | `#000000` |
| Database/Data Store | `#438DD5` | `#3C7FC0` | `#FFFFFF` |

### C4 Shape Styles

```xml
<!-- Person (Actor) -->
<mxCell id="person-1" value="&lt;b&gt;User Name&lt;/b&gt;&lt;br&gt;[Person]&lt;br&gt;&lt;br&gt;&lt;font style='font-size:10px'&gt;Description of the user&lt;/font&gt;"
  style="shape=umlActor;verticalLabelPosition=bottom;verticalAlign=top;html=1;outlineConnect=0;fillColor=#08427B;strokeColor=#052E56;fontColor=#FFFFFF;labelPosition=center;align=center;spacingTop=-60;spacing=0;fontSize=11;shadow=1;textShadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="100" y="200" width="80" height="120" as="geometry"/>
</mxCell>

<!-- Software System (Internal) - Main -->
<mxCell id="system-main" value="&lt;b&gt;System Name&lt;/b&gt;&lt;br&gt;[Software System]&lt;br&gt;&lt;br&gt;&lt;font style='font-size:10px'&gt;Description of the system&lt;/font&gt;"
  style="rounded=1;whiteSpace=wrap;html=1;fillColor=#438DD5;strokeColor=#3C7FC0;strokeWidth=3;fontColor=#FFFFFF;verticalAlign=middle;fontSize=11;shadow=1;textShadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="400" y="300" width="300" height="200" as="geometry"/>
</mxCell>

<!-- Software System (Internal) - Supporting -->
<mxCell id="system-support" value="&lt;b&gt;Supporting System&lt;/b&gt;&lt;br&gt;[Software System]&lt;br&gt;&lt;br&gt;&lt;font style='font-size:10px'&gt;Description&lt;/font&gt;"
  style="rounded=1;whiteSpace=wrap;html=1;fillColor=#438DD5;strokeColor=#3C7FC0;strokeWidth=2;fontColor=#FFFFFF;verticalAlign=middle;fontSize=11;shadow=1;textShadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="100" y="300" width="180" height="100" as="geometry"/>
</mxCell>

<!-- External System -->
<mxCell id="system-external" value="&lt;b&gt;External System&lt;/b&gt;&lt;br&gt;[External System]&lt;br&gt;&lt;br&gt;&lt;font style='font-size:10px'&gt;Description&lt;/font&gt;"
  style="rounded=1;whiteSpace=wrap;html=1;fillColor=#999999;strokeColor=#8A8A8A;strokeWidth=2;fontColor=#FFFFFF;verticalAlign=middle;fontSize=11;shadow=1;textShadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="800" y="300" width="200" height="100" as="geometry"/>
</mxCell>

<!-- Container -->
<mxCell id="container-1" value="&lt;b&gt;Container Name&lt;/b&gt;&lt;br&gt;[Container: Technology]&lt;br&gt;&lt;br&gt;&lt;font style='font-size:10px'&gt;Description&lt;/font&gt;"
  style="rounded=1;whiteSpace=wrap;html=1;fillColor=#438DD5;strokeColor=#3C7FC0;strokeWidth=2;fontColor=#FFFFFF;verticalAlign=middle;fontSize=11;shadow=1;textShadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="400" y="300" width="180" height="110" as="geometry"/>
</mxCell>

<!-- Database Container -->
<mxCell id="container-db" value="&lt;b&gt;Database Name&lt;/b&gt;&lt;br&gt;[Container: PostgreSQL]&lt;br&gt;&lt;br&gt;&lt;font style='font-size:10px'&gt;Description&lt;/font&gt;"
  style="shape=cylinder3;whiteSpace=wrap;html=1;boundedLbl=1;backgroundOutline=1;size=15;fillColor=#438DD5;strokeColor=#3C7FC0;strokeWidth=2;fontColor=#FFFFFF;verticalAlign=middle;fontSize=11;shadow=1;textShadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="700" y="300" width="170" height="130" as="geometry"/>
</mxCell>

<!-- Component -->
<mxCell id="component-1" value="&lt;b&gt;Component Name&lt;/b&gt;&lt;br&gt;[Component: Type]&lt;br&gt;&lt;br&gt;&lt;font style='font-size:10px'&gt;Description&lt;/font&gt;"
  style="rounded=1;whiteSpace=wrap;html=1;fillColor=#85BBF0;strokeColor=#5A9BD5;strokeWidth=2;fontColor=#000000;verticalAlign=middle;fontSize=11;shadow=1;textShadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="400" y="300" width="180" height="100" as="geometry"/>
</mxCell>
```

### C4 Boundary Containers

```xml
<!-- System Boundary (for Container diagrams) -->
<mxCell id="boundary-system" value="System Name [Software System]"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=none;strokeColor=#3C7FC0;strokeWidth=4;dashed=1;dashPattern=8 8;verticalAlign=top;align=left;spacingLeft=20;spacingTop=10;fontSize=16;fontStyle=1;fontColor=#3C7FC0;shadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="300" y="150" width="1400" height="900" as="geometry"/>
</mxCell>

<!-- Container Boundary (for Component diagrams) -->
<mxCell id="boundary-container" value="Container Name [Container: Technology]"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=none;strokeColor=#3C7FC0;strokeWidth=3;dashed=1;dashPattern=8 8;verticalAlign=top;align=left;spacingLeft=20;spacingTop=10;fontSize=14;fontStyle=1;fontColor=#3C7FC0;shadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="280" y="120" width="1100" height="700" as="geometry"/>
</mxCell>
```

### C4 Stroke Width Hierarchy

| Element | Stroke Width | Use Case |
|---------|--------------|----------|
| System Boundary | `strokeWidth=4` | Top-level boundary in Container diagram |
| Container Boundary | `strokeWidth=3` | Container boundary in Component diagram |
| Main System | `strokeWidth=3` | The central system being documented |
| Supporting Elements | `strokeWidth=2` | Other systems, containers, components |
| All Edges | `strokeWidth=2` | Consistent for all connections |

### C4 Arrow Styles

**CRITICAL: Flow Animation Rules**

| Arrow Type | Flow Animation | Style |
|------------|----------------|-------|
| Unidirectional (single arrow) | `flowAnimation=1` | Data flows one direction |
| Bidirectional (two arrows) | **NO flowAnimation** | Read/write, request/response |
| Async/Subscribe | **NO flowAnimation** | Polling, event consumption |

```xml
<!-- Unidirectional Arrow (WITH flowAnimation) -->
<mxCell id="rel-1" value="API calls [REST]"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#707070;strokeWidth=2;fontColor=#707070;fontSize=9;shadow=1;textShadow=1;flowAnimation=1;endArrow=classic;endFill=1;"
  parent="1" source="source-id" target="target-id" edge="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Bidirectional Arrow (NO flowAnimation) -->
<mxCell id="rel-db" value="SQL"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#707070;strokeWidth=2;fontColor=#707070;fontSize=9;shadow=1;textShadow=1;startArrow=classic;startFill=1;endArrow=classic;endFill=1;"
  parent="1" source="service-id" target="database-id" edge="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Async/Subscribe Arrow (NO flowAnimation) -->
<mxCell id="rel-async" value="Subscribes"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#707070;strokeWidth=2;fontColor=#707070;fontSize=9;shadow=1;textShadow=1;endArrow=async;endFill=1;"
  parent="1" source="consumer-id" target="broker-id" edge="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### C4 Arrow Type Quick Reference

| Connection Type | Arrow Style | flowAnimation |
|-----------------|-------------|---------------|
| HTTP/API Request | `endArrow=classic;endFill=1` | Yes |
| Data Flow | `endArrow=classic;endFill=1` | Yes |
| Publish Events | `endArrow=classic;endFill=1` | Yes |
| Database Read/Write | `startArrow=classic;endArrow=classic` | **No** |
| Cache Read/Write | `startArrow=classic;endArrow=classic` | **No** |
| External API Bidirectional | `startArrow=classic;endArrow=classic` | **No** |
| Subscribe/Poll | `endArrow=async;endFill=1` | **No** |
| Event Consumption | `endArrow=async;endFill=1` | **No** |

### C4 Edge Labels

Always label edges with the technology/protocol:

| Label Type | Examples |
|------------|----------|
| API Protocol | `REST`, `gRPC`, `GraphQL`, `HTTPS` |
| Database | `SQL`, `Reads/Writes` |
| Messaging | `Events`, `Subscribes`, `Publishes` |
| Integration | `ISO 20022`, `SWIFT`, `FTP` |

### C4 Legend Example

```xml
<!-- Legend for Context Diagram -->
<mxCell id="legend-context" value="Legend"
  style="swimlane;fontStyle=1;childLayout=stackLayout;horizontal=1;startSize=26;horizontalStack=0;resizeParent=1;resizeParentMax=0;resizeLast=0;collapsible=0;marginBottom=0;fillColor=#F5F5F5;strokeColor=#666666;strokeWidth=2;fontSize=12;shadow=1;"
  parent="1" vertex="1">
  <mxGeometry x="1200" y="150" width="300" height="156" as="geometry"/>
</mxCell>
<mxCell id="legend-person" value="&lt;table&gt;&lt;tr&gt;&lt;td style='background:#08427B;width:20px;'&gt;&lt;/td&gt;&lt;td&gt; Person (User/Actor)&lt;/td&gt;&lt;/tr&gt;&lt;/table&gt;"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=4;fontSize=11;"
  parent="legend-context" vertex="1">
  <mxGeometry y="26" width="300" height="26" as="geometry"/>
</mxCell>
<mxCell id="legend-system" value="&lt;table&gt;&lt;tr&gt;&lt;td style='background:#438DD5;width:20px;'&gt;&lt;/td&gt;&lt;td&gt; Software System (Internal)&lt;/td&gt;&lt;/tr&gt;&lt;/table&gt;"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=4;fontSize=11;"
  parent="legend-context" vertex="1">
  <mxGeometry y="52" width="300" height="26" as="geometry"/>
</mxCell>
<mxCell id="legend-external" value="&lt;table&gt;&lt;tr&gt;&lt;td style='background:#999999;width:20px;'&gt;&lt;/td&gt;&lt;td&gt; External System&lt;/td&gt;&lt;/tr&gt;&lt;/table&gt;"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=4;fontSize=11;"
  parent="legend-context" vertex="1">
  <mxGeometry y="78" width="300" height="26" as="geometry"/>
</mxCell>
<mxCell id="legend-rel" value="&lt;table&gt;&lt;tr&gt;&lt;td&gt;───▶&lt;/td&gt;&lt;td&gt; Uses / Depends On&lt;/td&gt;&lt;/tr&gt;&lt;/table&gt;"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=4;fontSize=11;"
  parent="legend-context" vertex="1">
  <mxGeometry y="104" width="300" height="26" as="geometry"/>
</mxCell>
<mxCell id="legend-bidir" value="&lt;table&gt;&lt;tr&gt;&lt;td&gt;◀──▶&lt;/td&gt;&lt;td&gt; Bidirectional (Read/Write)&lt;/td&gt;&lt;/tr&gt;&lt;/table&gt;"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=4;fontSize=11;"
  parent="legend-context" vertex="1">
  <mxGeometry y="130" width="300" height="26" as="geometry"/>
</mxCell>
```

### C4 Best Practices

1. **Each diagram level should have its own page** - Use multiple `<diagram>` elements
2. **Title every diagram** - Include diagram level and system name
3. **Use consistent positioning** - Left-to-right flow, users on left, external systems on right
4. **Include legends** - Explain colors and arrow types
5. **Label all relationships** - Protocol, technology, or action
6. **Main system uses thicker stroke** - `strokeWidth=3` vs `strokeWidth=2`
7. **Always add shadow** - `shadow=1;textShadow=1` on all elements
8. **HTML formatting for labels** - Use `<b>`, `<br>`, `<font>` for structured text

---

## Software Architecture Shape References

### UML Components
| Element | Shape Style |
|---------|-------------|
| Component | `shape=component` |
| Interface | `shape=lollipop` |
| Package | `shape=package` |
| Class | `shape=swimlane;fontStyle=1;align=center;verticalAlign=top` |
| Actor | `shape=umlActor` |
| Use Case | `shape=ellipse` |
| Artifact | `shape=artifact` |

### Microservices & APIs
| Component | Shape Style |
|-----------|-------------|
| API Gateway | `shape=mxgraph.aws4.api_gateway` |
| REST API | `rounded=1;arcSize=10;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| GraphQL | `rounded=1;arcSize=10;strokeColor=#E535AB;fillColor=#FCE4EC` |
| gRPC | `rounded=1;arcSize=10;strokeColor=#244c5a;fillColor=#e3f2fd` |
| Microservice | `rounded=1;arcSize=20;strokeColor=#666666;fillColor=#f5f5f5` |
| Lambda/Function | `shape=mxgraph.aws4.lambda_function` |

### Database Types
| Type | Shape Style |
|------|-------------|
| SQL Database | `shape=cylinder3;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| NoSQL Database | `shape=cylinder3;strokeColor=#82b366;fillColor=#d5e8d4` |
| Cache (Redis) | `shape=cylinder3;strokeColor=#d79b00;fillColor=#ffe6cc` |
| Message Queue | `shape=cylinder3;strokeColor=#b85450;fillColor=#f8cecc` |
| Data Warehouse | `shape=cylinder3;strokeColor=#9673a6;fillColor=#e1d5e7` |
| Time Series DB | `shape=cylinder3;strokeColor=#006EAF;fillColor=#1ba1e2` |

### Integration Patterns
| Pattern | Shape Style |
|---------|-------------|
| Message Queue | `shape=mxgraph.aws4.sqs` |
| Event Bus | `shape=mxgraph.aws4.eventbridge` |
| ESB | `rounded=0;strokeColor=#d6b656;fillColor=#fff2cc` |
| Pub/Sub | `shape=mxgraph.gcp2.cloud_pubsub` |
| Stream Processing | `shape=mxgraph.aws4.kinesis` |
| Service Mesh | `rounded=1;dashed=1;strokeColor=#666666;fillColor=none` |

---

## DevOps & CI/CD Shape References

### CI/CD Tools
| Tool | Shape/Style |
|------|-------------|
| Jenkins | `shape=mxgraph.sysml.package;strokeColor=#D33833;fillColor=#F4D9D0` |
| GitHub Actions | `rounded=1;strokeColor=#24292e;fillColor=#f6f8fa` |
| GitLab CI | `rounded=1;strokeColor=#FC6D26;fillColor=#FFF7F0` |
| ArgoCD | `rounded=1;strokeColor=#EF7B4D;fillColor=#FFF5F0` |
| Terraform | `rounded=1;strokeColor=#7B42BC;fillColor=#F5F0FF` |
| Ansible | `rounded=1;strokeColor=#EE0000;fillColor=#FFF0F0` |

### Monitoring & Observability
| Tool | Shape/Style |
|------|-------------|
| Prometheus | `rounded=1;strokeColor=#E6522C;fillColor=#FFF5F0` |
| Grafana | `rounded=1;strokeColor=#F46800;fillColor=#FFF8F0` |
| ELK Stack | `rounded=1;strokeColor=#005571;fillColor=#F0F8FF` |
| Datadog | `rounded=1;strokeColor=#632CA6;fillColor=#F8F0FF` |
| New Relic | `rounded=1;strokeColor=#008C99;fillColor=#F0FFFF` |
| Jaeger | `rounded=1;strokeColor=#66CFE3;fillColor=#F0FFFF` |

---

## Database Shape References

### Relational Databases (SQL)
| Database | Shape/Style |
|----------|-------------|
| Generic SQL DB | `shape=cylinder3;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| PostgreSQL | `shape=cylinder3;strokeColor=#336791;fillColor=#E8F4F8` |
| MySQL | `shape=cylinder3;strokeColor=#4479A1;fillColor=#E8F4FC` |
| SQL Server | `shape=cylinder3;strokeColor=#CC2927;fillColor=#FCE8E8` |
| Oracle | `shape=cylinder3;strokeColor=#F80000;fillColor=#FFF0F0` |
| MariaDB | `shape=cylinder3;strokeColor=#003545;fillColor=#E8F0F4` |

### NoSQL Databases
| Database | Shape/Style |
|----------|-------------|
| Generic NoSQL | `shape=cylinder3;strokeColor=#82b366;fillColor=#d5e8d4` |
| MongoDB | `shape=cylinder3;strokeColor=#4DB33D;fillColor=#E8F8E8` |
| Cassandra | `shape=cylinder3;strokeColor=#1287B1;fillColor=#E8F4FC` |
| CouchDB | `shape=cylinder3;strokeColor=#E42528;fillColor=#FCE8E8` |
| Neo4j (Graph) | `shape=cylinder3;strokeColor=#008CC1;fillColor=#E8F8FC` |
| Redis | `shape=cylinder3;strokeColor=#DC382D;fillColor=#FCE8E8` |
| Memcached | `shape=cylinder3;strokeColor=#4E9A06;fillColor=#E8F8E8` |

### Cloud-Managed Databases
| Service | Shape Style |
|---------|-------------|
| AWS RDS | `shape=mxgraph.aws4.rds` |
| AWS DynamoDB | `shape=mxgraph.aws4.dynamodb` |
| AWS Aurora | `shape=mxgraph.aws4.aurora` |
| AWS Redshift | `shape=mxgraph.aws4.redshift` |
| AWS DocumentDB | `shape=mxgraph.aws4.documentdb` |
| AWS Neptune | `shape=mxgraph.aws4.neptune` |
| Azure SQL | `shape=mxgraph.azure.databases.sql_databases` |
| Azure Cosmos DB | `shape=mxgraph.azure.databases.azure_cosmos_db` |
| GCP Cloud SQL | `shape=mxgraph.gcp2.cloud_sql` |
| GCP Spanner | `shape=mxgraph.gcp2.cloud_spanner` |
| GCP Firestore | `shape=mxgraph.gcp2.cloud_firestore` |
| GCP Bigtable | `shape=mxgraph.gcp2.cloud_bigtable` |

### Data Warehouse & Analytics
| Service | Shape Style |
|---------|-------------|
| Data Warehouse | `shape=cylinder3;strokeColor=#9673a6;fillColor=#e1d5e7` |
| AWS Redshift | `shape=mxgraph.aws4.redshift` |
| GCP BigQuery | `shape=mxgraph.gcp2.bigquery` |
| Azure Synapse | `shape=mxgraph.azure.databases.azure_synapse_analytics` |
| Snowflake | `shape=cylinder3;strokeColor=#29B5E8;fillColor=#E8F8FC` |
| Databricks | `shape=cylinder3;strokeColor=#FF3621;fillColor=#FFF0F0` |

---

## Search Engine & Analytics Shape References

### Search Engines
| Service | Shape/Style |
|---------|-------------|
| Elasticsearch | `rounded=1;strokeColor=#005571;fillColor=#F0F8FF` |
| OpenSearch | `rounded=1;strokeColor=#005EB8;fillColor=#E8F4FC` |
| Apache Solr | `rounded=1;strokeColor=#D9411E;fillColor=#FCE8E4` |
| Algolia | `rounded=1;strokeColor=#5468FF;fillColor=#F0F4FF` |
| Meilisearch | `rounded=1;strokeColor=#FF5CAA;fillColor=#FFF0F8` |
| Typesense | `rounded=1;strokeColor=#5928ED;fillColor=#F8F0FF` |

### Cloud Search Services
| Service | Shape Style |
|---------|-------------|
| AWS OpenSearch | `shape=mxgraph.aws4.elasticsearch_service` |
| AWS CloudSearch | `shape=mxgraph.aws4.cloudsearch` |
| Azure Cognitive Search | `shape=mxgraph.azure.ai_machine_learning.cognitive_services` |
| GCP Cloud Search | `shape=mxgraph.gcp2.cloud_search` |

### Log & Analytics Platforms
| Service | Shape/Style |
|---------|-------------|
| Kibana | `rounded=1;strokeColor=#E8488B;fillColor=#FFF0F8` |
| Logstash | `rounded=1;strokeColor=#FEC514;fillColor=#FFFCF0` |
| Splunk | `rounded=1;strokeColor=#65A637;fillColor=#F0FCF0` |
| Graylog | `rounded=1;strokeColor=#FF3633;fillColor=#FFF0F0` |
| Fluentd | `rounded=1;strokeColor=#0E83C8;fillColor=#E8F4FC` |
| Vector | `rounded=1;strokeColor=#7B42BC;fillColor=#F8F0FF` |

---

## Data Workflow & ETL Shape References

### Data Pipeline & Orchestration
| Tool | Shape/Style |
|------|-------------|
| Apache Airflow | `rounded=1;strokeColor=#017CEE;fillColor=#E8F4FF` |
| Prefect | `rounded=1;strokeColor=#2E90FA;fillColor=#E8F4FF` |
| Dagster | `rounded=1;strokeColor=#4F43DD;fillColor=#F0F0FF` |
| Luigi | `rounded=1;strokeColor=#2BBA00;fillColor=#E8FCE8` |
| dbt | `rounded=1;strokeColor=#FF694B;fillColor=#FFF0EC` |
| Mage | `rounded=1;strokeColor=#6B50FF;fillColor=#F0ECFF` |

### Stream Processing
| Tool | Shape Style |
|------|-------------|
| Apache Kafka | `rounded=1;strokeColor=#231F20;fillColor=#F5F5F5` |
| Apache Spark | `rounded=1;strokeColor=#E25A1C;fillColor=#FFF0EC` |
| Apache Flink | `rounded=1;strokeColor=#E6526F;fillColor=#FFF0F4` |
| Apache Storm | `rounded=1;strokeColor=#2379BF;fillColor=#E8F4FF` |
| Apache Beam | `rounded=1;strokeColor=#FF6F00;fillColor=#FFF8F0` |
| AWS Kinesis | `shape=mxgraph.aws4.kinesis` |
| GCP Dataflow | `shape=mxgraph.gcp2.dataflow` |
| Azure Stream Analytics | `shape=mxgraph.azure.analytics.stream_analytics_jobs` |

### ETL & Data Integration
| Tool | Shape/Style |
|------|-------------|
| Apache NiFi | `rounded=1;strokeColor=#728E9B;fillColor=#F0F4F8` |
| Talend | `rounded=1;strokeColor=#FF6D70;fillColor=#FFF0F0` |
| Informatica | `rounded=1;strokeColor=#FF4D00;fillColor=#FFF4F0` |
| Fivetran | `rounded=1;strokeColor=#0073FF;fillColor=#E8F4FF` |
| Airbyte | `rounded=1;strokeColor=#615EFF;fillColor=#F0F0FF` |
| Stitch | `rounded=1;strokeColor=#FFCC00;fillColor=#FFFCF0` |

### Cloud ETL/Data Services
| Service | Shape Style |
|---------|-------------|
| AWS Glue | `shape=mxgraph.aws4.glue` |
| AWS Data Pipeline | `shape=mxgraph.aws4.data_pipeline` |
| AWS EMR | `shape=mxgraph.aws4.emr` |
| GCP Dataproc | `shape=mxgraph.gcp2.cloud_dataproc` |
| GCP Cloud Data Fusion | `shape=mxgraph.gcp2.cloud_data_fusion` |
| Azure Data Factory | `shape=mxgraph.azure.analytics.data_factory` |
| Azure HDInsight | `shape=mxgraph.azure.analytics.hdinsight_clusters` |

### Data Workflow Symbols
| Symbol | Shape/Style | Use Case |
|--------|-------------|----------|
| Source | `shape=parallelogram;strokeColor=#82b366;fillColor=#d5e8d4` | Data source input |
| Transform | `shape=process;strokeColor=#6c8ebf;fillColor=#dae8fc` | Data transformation step |
| Sink/Target | `shape=cylinder3;strokeColor=#9673a6;fillColor=#e1d5e7` | Data destination |
| Decision | `shape=rhombus;strokeColor=#d6b656;fillColor=#fff2cc` | Branch/conditional |
| Queue/Buffer | `shape=mxgraph.aws4.sqs` | Message queue/buffer |

---

## Workflow & Process Diagram Shape References

### BPMN (Business Process Model and Notation)
| Element | Shape/Style |
|---------|-------------|
| Start Event | `shape=mxgraph.bpmn.shape;perimeter=ellipsePerimeter;symbol=general;strokeColor=#228B22;fillColor=#d5e8d4` |
| End Event | `shape=mxgraph.bpmn.shape;perimeter=ellipsePerimeter;symbol=terminate;strokeColor=#B22222;fillColor=#f8cecc` |
| Task | `rounded=1;arcSize=10;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| User Task | `shape=mxgraph.bpmn.shape;perimeter=rectanglePerimeter;symbol=user_task;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Service Task | `shape=mxgraph.bpmn.shape;perimeter=rectanglePerimeter;symbol=service_task;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Script Task | `shape=mxgraph.bpmn.shape;perimeter=rectanglePerimeter;symbol=script_task;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Gateway (Exclusive) | `shape=mxgraph.bpmn.shape;perimeter=rhombusPerimeter;symbol=exclusiveGw;strokeColor=#d6b656;fillColor=#fff2cc` |
| Gateway (Parallel) | `shape=mxgraph.bpmn.shape;perimeter=rhombusPerimeter;symbol=parallelGw;strokeColor=#d6b656;fillColor=#fff2cc` |
| Gateway (Inclusive) | `shape=mxgraph.bpmn.shape;perimeter=rhombusPerimeter;symbol=inclusiveGw;strokeColor=#d6b656;fillColor=#fff2cc` |
| Subprocess | `rounded=1;arcSize=10;strokeColor=#6c8ebf;fillColor=#dae8fc;strokeWidth=2` |
| Pool/Lane | `swimlane;horizontal=1;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Message Flow | `dashed=1;strokeColor=#666666;endArrow=open;endFill=0` |
| Sequence Flow | `strokeColor=#232F3E;endArrow=classic;endFill=1` |

### Flowchart Shapes
| Element | Shape/Style |
|---------|-------------|
| Start/End (Oval) | `shape=ellipse;strokeColor=#82b366;fillColor=#d5e8d4` |
| Process (Rectangle) | `rounded=0;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Decision (Diamond) | `shape=rhombus;strokeColor=#d6b656;fillColor=#fff2cc` |
| Input/Output | `shape=parallelogram;strokeColor=#82b366;fillColor=#d5e8d4` |
| Document | `shape=document;strokeColor=#9673a6;fillColor=#e1d5e7` |
| Database | `shape=cylinder3;strokeColor=#9673a6;fillColor=#e1d5e7` |
| Manual Operation | `shape=trapezoid;strokeColor=#b85450;fillColor=#f8cecc` |
| Predefined Process | `shape=process;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Connector | `shape=ellipse;strokeColor=#666666;fillColor=#f5f5f5` |
| Delay | `shape=mxgraph.flowchart.delay;strokeColor=#d6b656;fillColor=#fff2cc` |
| Merge | `shape=triangle;direction=south;strokeColor=#d6b656;fillColor=#fff2cc` |

### State Machine / Statechart
| Element | Shape/Style |
|---------|-------------|
| Initial State | `shape=ellipse;fillColor=#000000;strokeColor=#000000` |
| State | `rounded=1;arcSize=20;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Final State | `shape=doubleEllipse;fillColor=#000000;strokeColor=#000000` |
| Composite State | `rounded=1;arcSize=10;strokeColor=#6c8ebf;fillColor=#dae8fc;strokeWidth=2` |
| Fork/Join | `rounded=0;fillColor=#000000;strokeColor=#000000` |
| Choice | `shape=rhombus;strokeColor=#d6b656;fillColor=#fff2cc` |
| History | `shape=ellipse;strokeColor=#6c8ebf;fillColor=none` |
| Transition | `strokeColor=#232F3E;endArrow=classic;endFill=1` |

### Sequence Diagram
| Element | Shape/Style |
|---------|-------------|
| Actor | `shape=umlActor;strokeColor=#232F3E;fillColor=#f5f5f5` |
| Lifeline | `shape=umlLifeline;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Activation | `rounded=0;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Synchronous Message | `strokeColor=#232F3E;endArrow=block;endFill=1` |
| Asynchronous Message | `strokeColor=#232F3E;endArrow=open;endFill=0` |
| Return Message | `dashed=1;strokeColor=#666666;endArrow=open;endFill=0` |
| Self Message | `edgeStyle=orthogonalEdgeStyle;curved=1;strokeColor=#232F3E` |

### Activity Diagram
| Element | Shape/Style |
|---------|-------------|
| Initial Node | `shape=ellipse;fillColor=#000000;strokeColor=#000000` |
| Activity | `rounded=1;arcSize=20;strokeColor=#6c8ebf;fillColor=#dae8fc` |
| Decision | `shape=rhombus;strokeColor=#d6b656;fillColor=#fff2cc` |
| Fork Node | `rounded=0;fillColor=#000000;strokeColor=#000000` |
| Join Node | `rounded=0;fillColor=#000000;strokeColor=#000000` |
| Final Node | `shape=doubleEllipse;fillColor=#000000;strokeColor=#000000` |
| Flow Final | `shape=ellipse;strokeColor=#B22222;fillColor=#f8cecc` |
| Object Node | `rounded=0;strokeColor=#9673a6;fillColor=#e1d5e7` |
| Swimlane | `swimlane;horizontal=0;strokeColor=#6c8ebf;fillColor=none` |

### General Workflow Colors
| Purpose | Fill Color | Stroke Color |
|---------|------------|--------------|
| Start/Success | `#d5e8d4` | `#82b366` |
| Process/Task | `#dae8fc` | `#6c8ebf` |
| Decision/Gateway | `#fff2cc` | `#d6b656` |
| Error/End | `#f8cecc` | `#b85450` |
| Data/Storage | `#e1d5e7` | `#9673a6` |
| External/Manual | `#f5f5f5` | `#666666` |

---

## Database ERD (Entity-Relationship Diagrams)

Guidelines for creating database schema diagrams with table relationships, including multi-database architectures (SQL + NoSQL + Cache + Search).

### Table Shape Structure (Swimlane with Separate Row Cells)

Use swimlane container with separate child mxCell elements for each row. This enables arrows to connect directly to specific columns (FK → PK mappings).

```xml
<!-- Table container (swimlane) -->
<mxCell id="tbl-users" value="users"
  style="swimlane;fontStyle=1;childLayout=stackLayout;horizontal=1;startSize=30;horizontalStack=0;resizeParent=1;resizeParentMax=0;resizeLast=0;collapsible=0;marginBottom=0;swimlaneFillColor=[darkest tone];fillColor=[darker tone];strokeColor=[medium tone];strokeWidth=3;shadow=1;fontSize=12;fontColor=#FFFFFF;"
  parent="pg-group" vertex="1">
  <mxGeometry x="40" y="60" width="250" height="130" as="geometry"/>
</mxCell>

<!-- Row with HTML table for column separation -->
<mxCell id="tbl-users-id" value="&lt;table style='width:100%;border-collapse:collapse'&gt;&lt;tr&gt;&lt;td style='width:30px;color:#D4AF37;font-weight:bold'&gt;PK&lt;/td&gt;&lt;td style='width:110px'&gt;id&lt;/td&gt;&lt;td style='color:#666'&gt;UUID&lt;/td&gt;&lt;/tr&gt;&lt;/table&gt;"
  style="text;strokeColor=[lighter tone];strokeWidth=3;fillColor=[light tone];align=left;verticalAlign=middle;spacingLeft=4;spacingRight=4;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;html=1;fontSize=10;"
  parent="tbl-users" vertex="1">
  <mxGeometry y="30" width="250" height="24" as="geometry"/>
</mxCell>
```

### Key Benefits of Swimlane Format

1. **Column-specific arrows**: Edges can connect directly from PK row to FK row
2. **Separate columns**: HTML table in each row provides visual column separation (PK/FK | Column Name | Data Type)
3. **Styled header**: Dark header background with white text for table name
4. **Row separators**: Each row has stroke for visual line separation
5. **Connection points**: `points=[[0,0.5],[1,0.5]]` enables left/right edge connections

### Swimlane Container Style

Key attributes:
- `swimlane` - Container shape type
- `childLayout=stackLayout` - Stack children vertically
- `startSize=30` - Header height (30px)
- `swimlaneFillColor` - Header background (use darkest tone)
- `fillColor` - Table body fill (use darker tone)
- `strokeColor` - Border color (use medium tone)
- `strokeWidth=3` - Border thickness
- `fontColor=#FFFFFF` - White header text
- `collapsible=0` - Prevent collapse
- `fontStyle=1` - Bold header text

### Row Cell Style

Key attributes:
- `text` - Text cell shape
- `html=1` - Enable HTML content for column separation
- `strokeColor` - Row separator line (use lighter tone)
- `strokeWidth=3` - Row line thickness
- `fillColor` - Row background (light tone, alternating)
- `points=[[0,0.5],[1,0.5]]` - Connection points on left and right
- `portConstraint=eastwest` - Edges connect horizontally

### Color Tone Hierarchy

| Element | Tone | Purpose |
|---------|------|---------|
| Header background (`swimlaneFillColor`) | Darkest | Table title background |
| Table body (`fillColor`) | Darker | Swimlane body area |
| Border (`strokeColor`) | Medium | Table outline |
| Row lines (`strokeColor` on rows) | Lighter | Row separators |
| Row fill (`fillColor` on rows) | Lightest | Row backgrounds |

### Row Styling by Type

| Row Type | Background | Text Style |
|----------|------------|------------|
| Primary Key (PK) | Light tone (alternating) | Gold text, bold (`color:#D4AF37;font-weight:bold`) |
| Foreign Key (FK) | Light blue tone | Blue text, bold (`color:#1976D2;font-weight:bold`) |
| Normal (odd) | Light tone | Default |
| Normal (even) | White | Default |

### HTML Table Column Structure

Each row uses an HTML table for column separation:

```html
<table style='width:100%;border-collapse:collapse'>
  <tr>
    <td style='width:30px;color:#D4AF37;font-weight:bold'>PK</td>
    <td style='width:110px'>column_name</td>
    <td style='color:#666'>DATA_TYPE</td>
  </tr>
</table>
```

Column widths:
- Column 1 (30px): Key type (PK/FK or empty)
- Column 2 (110px): Column name
- Column 3 (auto): Data type (gray text)

### Connecting Arrows to Specific Columns

Arrows connect directly from PK cell to FK cell:

```xml
<!-- agencies.id → users.agency_id (1:N) -->
<mxCell id="rel-agencies-users"
  style="edgeStyle=entityRelationEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERmany;endFill=0;shadow=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;"
  edge="1" parent="1" source="tbl-agencies-id" target="tbl-users-agency_id">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

Key points:
- `source="tbl-agencies-id"` - Points to the PK row cell (not the table)
- `target="tbl-users-agency_id"` - Points to the FK row cell (not the table)
- `exitX=1;exitY=0.5` - Exit from right side of source row
- `entryX=0;entryY=0.5` - Enter from left side of target row

### Database Type Colors

| Database Type | Fill Color | Stroke Color | Use Case |
|---------------|------------|--------------|----------|
| PostgreSQL | `#E8F4F8` | `#336791` | Primary relational DB |
| MySQL | `#E8F4FC` | `#4479A1` | Relational DB |
| MongoDB | `#E8F8E8` | `#4DB33D` | Document store |
| Redis | `#FCE8E8` | `#DC382D` | Cache layer |
| Elasticsearch | `#FFF0F0` | `#b85450` | Full-text search |
| Central/Core Table | `#FFF2CC` | `#D6B656` | Highlight key entity |

### Database Group Containers

```xml
<!-- PostgreSQL Group -->
<mxCell id="pg-group" value="PostgreSQL (Primary Database)"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;strokeWidth=3;verticalAlign=top;align=left;spacingLeft=15;spacingTop=8;fontStyle=1;fontSize=14;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="40" width="1500" height="1100" as="geometry"/>
</mxCell>

<!-- MongoDB Group -->
<mxCell id="mongo-group" value="MongoDB (Document Store)"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;strokeWidth=3;verticalAlign=top;align=left;spacingLeft=15;spacingTop=8;fontStyle=1;fontSize=14;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="1600" y="40" width="400" height="420" as="geometry"/>
</mxCell>

<!-- Redis Group -->
<mxCell id="redis-group" value="Redis (Cache Layer)"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#ffe6cc;strokeColor=#d79b00;strokeWidth=3;verticalAlign=top;align=left;spacingLeft=15;spacingTop=8;fontStyle=1;fontSize=14;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="1600" y="500" width="400" height="220" as="geometry"/>
</mxCell>
```

### ER Relationship Arrows

Use ER-specific arrows for database relationships:

| Relationship | Start Arrow | End Arrow | Style |
|--------------|-------------|-----------|-------|
| One-to-Many (1:N) | `ERone` | `ERmany` | `startArrow=ERone;startFill=0;endArrow=ERmany;endFill=0` |
| One-to-One (1:1) | `ERone` | `ERone` | `startArrow=ERone;startFill=0;endArrow=ERone;endFill=0` |
| One-to-Zero/One (1:0..1) | `ERone` | `ERzeroToOne` | `startArrow=ERone;startFill=0;endArrow=ERzeroToOne;endFill=0` |
| Many-to-Many (M:N) | `ERmany` | `ERmany` | `startArrow=ERmany;startFill=0;endArrow=ERmany;endFill=0` |
| Zero/Many-to-Many (0..*:N) | `ERzeroToMany` | `ERmany` | `startArrow=ERzeroToMany;startFill=0;endArrow=ERmany;endFill=0` |

**CRITICAL: ER Arrows Must Connect Horizontally Only (Side to Side)**

ER relationship arrows should ONLY connect horizontally (left ↔ right), NEVER vertically (top ↔ bottom). If tables need vertical relationships, reposition them so arrows can connect side to side.

| Connection Direction | Edge Style | Required |
|---------------------|------------|----------|
| Horizontal (left ↔ right) | `entityRelationEdgeStyle;rounded=0` | **YES - Always use** |
| Vertical (top ↔ bottom) | N/A | **NO - Reposition tables instead** |

**Connection Points for Horizontal Arrows:**
- Exit from left side: `exitX=0;exitY=0.5`
- Exit from right side: `exitX=1;exitY=0.5`
- Enter from left side: `entryX=0;entryY=0.5`
- Enter from right side: `entryX=1;entryY=0.5`

**CRITICAL: Arrow Trails Must NEVER Cross Table Content**

Arrow paths must route through empty gaps between tables, never crossing through any table's visual area. Use `orthogonalEdgeStyle` with explicit waypoints to control exact routing.

**Waypoint Strategy:**
1. Identify gaps between table columns (e.g., x=420 between col1 and col2)
2. Add waypoints at gap x-coordinates for vertical segments
3. Ensure horizontal segments stay outside table y-ranges

| Gap Location | Waypoint X | Use Case |
|--------------|------------|----------|
| Left of all tables | x < leftmost table | Left perimeter routing |
| Between column 1 and 2 | x = 420 (example) | Connect col1 ↔ col2 tables |
| Between column 2 and 3 | x = 760-790 (example) | Connect col2 tables vertically |
| Right of all tables | x > rightmost table | Right perimeter routing |

```xml
<!-- Direct horizontal connection (no waypoints needed) -->
<mxCell id="rel-users-orders"
  style="edgeStyle=entityRelationEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERmany;endFill=0;shadow=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;"
  edge="1" parent="1" source="tbl-users-id" target="tbl-orders-user_id">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Same-column connection with waypoints routing through gap -->
<mxCell id="rel-properties-listings"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERmany;endFill=0;shadow=1;exitX=1;exitY=0.5;entryX=1;entryY=0.5;"
  edge="1" parent="1" source="tbl-properties-id" target="tbl-listings-property_id">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="760" y="512"/>
      <mxPoint x="760" y="1014"/>
    </Array>
  </mxGeometry>
</mxCell>

<!-- Cross-column connection with waypoints avoiding other tables -->
<mxCell id="rel-properties-favorites"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERmany;endFill=0;shadow=1;exitX=1;exitY=0.5;entryX=1;entryY=0.5;"
  edge="1" parent="1" source="tbl-properties-id" target="tbl-favorites-property_id">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="1160" y="512"/>
      <mxPoint x="1160" y="178"/>
    </Array>
  </mxGeometry>
</mxCell>
```

### Cross-Database Sync Arrows

For data synchronization between different database types:

| Sync Type | Stroke Color | Style |
|-----------|--------------|-------|
| Index Sync (→ Elasticsearch) | `#9673a6` | `dashed=1;dashPattern=8 8;flowAnimation=1` |
| Cache Sync (→ Redis) | `#d79b00` | `dashed=1;dashPattern=8 8;flowAnimation=1` |
| Event Stream (→ MongoDB) | `#82b366` | `dashed=1;dashPattern=8 8;flowAnimation=1` |

```xml
<!-- Index sync to Elasticsearch -->
<mxCell id="sync-pg-es" value="Index Sync"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#9673a6;strokeWidth=2;dashed=1;dashPattern=8 8;endArrow=classic;endFill=1;shadow=1;flowAnimation=1;fontSize=10;labelBackgroundColor=#ffffff;"
  edge="1" parent="1" source="tbl-properties" target="idx-property-search">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="1570" y="495"/>
      <mxPoint x="1570" y="900"/>
    </Array>
  </mxGeometry>
</mxCell>

<!-- Cache sync to Redis -->
<mxCell id="sync-pg-redis" value="Cache"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#d79b00;strokeWidth=2;dashed=1;dashPattern=8 8;endArrow=classic;endFill=1;shadow=1;flowAnimation=1;fontSize=10;labelBackgroundColor=#ffffff;"
  edge="1" parent="1" source="tbl-properties" target="cache-property">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### ERD Layout Best Practices

**CRITICAL: Spacing Requirements**

| Spacing Type | Minimum Distance | Recommended |
|--------------|------------------|-------------|
| Horizontal gap between tables | 100px | **120px** |
| Vertical gap between table rows | 80px | **100px** |
| Table width | 250px | 260-280px |
| Table height | Based on columns | ~270px for 10 columns |

**Row-Based Organization:**
```
Row 1: User Management    [agencies] → [users] → [favorites]
Row 2: Core Entities      [types] [locations] → [CENTRAL TABLE] ← [images] [features]
Row 3: Business Logic     [inquiries] ← [listings] → [appointments]
Row 4: Transactions       [transactions] → [contracts] [payments]
```

**Table Placement Rules:**
1. **Central/core table highlighted** - Use yellow (`#FFF2CC`) for the main entity
2. **Related tables adjacent** - Tables with FK relationships should be neighbors
3. **Flow direction** - Left-to-right for dependencies, top-to-bottom for hierarchy
4. **NoSQL/Cache on separate side** - Place non-SQL databases in separate column (right side)

**Avoiding Edge Crossings:**
1. Position related tables as direct neighbors
2. Route cross-database sync arrows around the perimeter (top/right edges)
3. Use waypoints for complex routing
4. Never route edges through table shapes

### ERD Legend Example

```xml
<!-- Legend container -->
<mxCell id="legend" value="Legend"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;strokeWidth=2;verticalAlign=top;align=left;spacingLeft=10;spacingTop=5;fontStyle=1;fontSize=12;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="1180" width="800" height="130" as="geometry"/>
</mxCell>

<!-- Legend items for database types -->
<mxCell id="legend-sql" value="PostgreSQL"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#E8F4F8;strokeColor=#336791;strokeWidth=2;shadow=1;fontSize=10;fontStyle=1;"
  vertex="1" parent="1">
  <mxGeometry x="60" y="1210" width="90" height="28" as="geometry"/>
</mxCell>

<!-- Legend edge for One-to-Many -->
<mxCell id="legend-one-many" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERmany;endFill=0;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="70" y="1270" as="sourcePoint"/>
    <mxPoint x="150" y="1270" as="targetPoint"/>
  </mxGeometry>
</mxCell>
```

---

## NoSQL / DynamoDB Data Model Diagrams

For NoSQL databases (DynamoDB, MongoDB, etc.), ERD diagrams show table/collection structures with partition keys, sort keys, and access patterns instead of traditional FK relationships.

### NoSQL Table Style

NoSQL tables use the same swimlane format but with different key indicators:

```xml
<!-- DynamoDB Table -->
<mxCell id="tbl-mr-identity" value="mr-identity"
  style="swimlane;fontStyle=1;childLayout=stackLayout;horizontal=1;startSize=30;horizontalStack=0;resizeParent=1;resizeParentMax=0;resizeLast=0;collapsible=0;marginBottom=0;swimlaneFillColor=#C925D1;fillColor=#E1D5E7;strokeColor=#9673A6;strokeWidth=3;shadow=1;fontSize=12;fontColor=#FFFFFF;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="100" width="280" height="280" as="geometry"/>
</mxCell>

<!-- Partition Key row -->
<mxCell id="mr-id-pk" value="🔑 mrId : String (PK)"
  style="text;strokeColor=#9673A6;fillColor=#F3E8FF;align=left;verticalAlign=middle;spacingLeft=10;spacingRight=4;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontStyle=1;fontSize=11;"
  vertex="1" parent="tbl-mr-identity">
  <mxGeometry y="30" width="280" height="25" as="geometry"/>
</mxCell>

<!-- TTL attribute row -->
<mxCell id="dev-ttl" value="⏱ TimeToExist : Number (TTL)"
  style="text;strokeColor=#9673A6;fillColor=#FFF2CC;align=left;verticalAlign=middle;spacingLeft=10;spacingRight=4;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontSize=10;fontStyle=2;"
  vertex="1" parent="tbl-mr-device">
  <mxGeometry y="205" width="280" height="25" as="geometry"/>
</mxCell>
```

### NoSQL Key Indicators

| Key Type | Style | Example |
|----------|-------|---------|
| Partition Key (PK) | 🔑 prefix, bold, light purple fill | `🔑 mrId : String (PK)` |
| Sort Key (SK) | 🔑 prefix, bold, light blue fill | `🔑 timestamp : Number (SK)` |
| TTL Attribute | ⏱ prefix, italic, yellow fill (#FFF2CC) | `⏱ TimeToExist : Number (TTL)` |
| GSI | 📇 prefix | `📇 email-index` |

### NoSQL Color Scheme by Domain

| Domain | Header Color | Fill Color | Stroke Color |
|--------|--------------|------------|--------------|
| User/Profile | `#C925D1` | `#E1D5E7` | `#9673A6` |
| Security/Rate Limit | `#DD344C` | `#F8CECC` | `#B85450` |
| Business Data | `#7AA116` | `#D5E8D4` | `#82B366` |
| Queue/Workflow | `#6C8EBF` | `#DAE8FC` | `#6C8EBF` |

### NoSQL Legend Example

```xml
<!-- Legend -->
<mxCell id="legend" value="Legend"
  style="swimlane;fontStyle=1;childLayout=stackLayout;horizontal=1;startSize=26;horizontalStack=0;resizeParent=1;resizeParentMax=0;resizeLast=0;collapsible=0;marginBottom=0;fillColor=#F5F5F5;strokeColor=#666666;strokeWidth=2;fontSize=12;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="620" width="280" height="182" as="geometry"/>
</mxCell>
<mxCell id="leg1" value="■ User/Profile Tables"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontColor=#9673A6;fontSize=11;fontStyle=1;"
  vertex="1" parent="legend">
  <mxGeometry y="26" width="280" height="26" as="geometry"/>
</mxCell>
<mxCell id="leg5" value="🔑 = Partition Key (PK)"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontColor=#333333;fontSize=11;"
  vertex="1" parent="legend">
  <mxGeometry y="130" width="280" height="26" as="geometry"/>
</mxCell>
<mxCell id="leg6" value="⏱ = TTL Attribute (Auto-expire)"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontColor=#333333;fontSize=11;"
  vertex="1" parent="legend">
  <mxGeometry y="156" width="280" height="26" as="geometry"/>
</mxCell>
```

### Access Patterns Box

For NoSQL, include an access patterns box showing query patterns:

```xml
<!-- Access Patterns -->
<mxCell id="patterns" value="Access Patterns"
  style="swimlane;fontStyle=1;childLayout=stackLayout;horizontal=1;startSize=26;horizontalStack=0;resizeParent=1;resizeParentMax=0;resizeLast=0;collapsible=0;marginBottom=0;fillColor=#F5F5F5;strokeColor=#666666;strokeWidth=2;fontSize=12;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="360" y="620" width="300" height="182" as="geometry"/>
</mxCell>
<mxCell id="pat1" value="• Get MR profile by mrId"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontSize=10;"
  vertex="1" parent="patterns">
  <mxGeometry y="26" width="300" height="22" as="geometry"/>
</mxCell>
```

---

## Conceptual ERD for NoSQL (Domain-Based)

When creating conceptual ERDs for NoSQL databases, group entities by domain and show logical relationships (even though NoSQL doesn't enforce FK constraints).

### Domain Container Style

```xml
<!-- Domain container using swimlane -->
<mxCell id="user-domain" value="User Domain"
  style="swimlane;startSize=0;strokeWidth=3;labelBorderColor=none;textShadow=1;labelBackgroundColor=default;fillColor=#E6F2FF;strokeColor=#6C8EBF;fontSize=14;fontStyle=1;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="80" width="500" height="400" as="geometry"/>
</mxCell>
```

### Logical Relationship Arrows for NoSQL

Use ERD arrows to show logical relationships (application-enforced, not database-enforced):

```xml
<!-- One-to-One relationship -->
<mxCell id="rel-emp-mr"
  style="edgeStyle=entityRelationEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#6C8EBF;strokeWidth=2;endArrow=ERone;endFill=0;startArrow=ERone;startFill=0;shadow=1;"
  edge="1" parent="1" source="employee" target="mr-profile">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- One-to-Many relationship -->
<mxCell id="rel-emp-dev"
  style="edgeStyle=entityRelationEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#6C8EBF;strokeWidth=2;endArrow=ERmany;endFill=0;startArrow=ERone;startFill=0;shadow=1;"
  edge="1" parent="1" source="employee" target="device">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### Conceptual ERD Legend

```xml
<!-- Relationship Legend -->
<mxCell id="legend" value="Relationship Legend"
  style="swimlane;fontStyle=1;childLayout=stackLayout;horizontal=1;startSize=26;horizontalStack=0;resizeParent=1;resizeParentMax=0;resizeLast=0;collapsible=0;marginBottom=0;fillColor=#F5F5F5;strokeColor=#666666;strokeWidth=2;fontSize=12;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="520" width="250" height="130" as="geometry"/>
</mxCell>
<mxCell id="leg1" value="──|──  One-to-One (1:1)"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontSize=11;"
  vertex="1" parent="legend">
  <mxGeometry y="26" width="250" height="26" as="geometry"/>
</mxCell>
<mxCell id="leg2" value="──|──&lt;  One-to-Many (1:N)"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontSize=11;"
  vertex="1" parent="legend">
  <mxGeometry y="52" width="250" height="26" as="geometry"/>
</mxCell>
<mxCell id="leg4" value="(FK) = Foreign Key (logical reference)"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontSize=11;fontStyle=2;"
  vertex="1" parent="legend">
  <mxGeometry y="104" width="250" height="26" as="geometry"/>
</mxCell>

<!-- Domain Legend -->
<mxCell id="domain-legend" value="Domain Colors"
  style="swimlane;fontStyle=1;childLayout=stackLayout;horizontal=1;startSize=26;horizontalStack=0;resizeParent=1;resizeParentMax=0;resizeLast=0;collapsible=0;marginBottom=0;fillColor=#F5F5F5;strokeColor=#666666;strokeWidth=2;fontSize=12;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="320" y="520" width="200" height="104" as="geometry"/>
</mxCell>
```

---

## Project Structure Diagrams

Project structure diagrams visualize the folder organization, module hierarchy, and codebase layout of a project. They use swimlane containers to group related directories and color-coding to distinguish between different technology types.

### Purpose

- Show overall codebase organization at a glance
- Distinguish between language-specific modules (Kotlin, Python, TypeScript, etc.)
- Highlight infrastructure and build configuration files
- Display module counts and key components per section

### Color Scheme by Technology

| Technology Type | Fill Color | Stroke Color | Header Color | Use For |
|----------------|------------|--------------|--------------|---------|
| Primary Language (Kotlin/Java) | `#DAE8FC` | `#6C8EBF` | `#6C8EBF` | Main language modules |
| Secondary Language (Python/JS) | `#FFF2CC` | `#D6B656` | `#D6B656` | Secondary language code |
| Infrastructure (Terraform/Docker) | `#D5E8D4` | `#82B366` | `#82B366` | IaC and DevOps files |
| Build/Config Files | `#F5F5F5` | `#666666` | `#666666` | Build configs, CI/CD |
| Shared/Common Modules | `#E1D5E7` | `#9673A6` | `#9673A6` | Shared libraries |
| Root/Overview | `#FFFFFF` | `#333333` | `#333333` | Root structure |

### Section Container Style

Use swimlane containers for major directory sections:

```xml
<!-- Kotlin Section Container -->
<mxCell id="kotlin-section" value="📁 kotlin/ (26 modules)"
  style="swimlane;startSize=30;strokeWidth=3;fillColor=#DAE8FC;strokeColor=#6C8EBF;fontSize=14;fontStyle=1;shadow=1;swimlaneFillColor=#DAE8FC;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="80" width="320" height="500" as="geometry"/>
</mxCell>

<!-- Python Section Container -->
<mxCell id="python-section" value="📁 python/ (35 modules)"
  style="swimlane;startSize=30;strokeWidth=3;fillColor=#FFF2CC;strokeColor=#D6B656;fontSize=14;fontStyle=1;shadow=1;swimlaneFillColor=#FFF2CC;"
  vertex="1" parent="1">
  <mxGeometry x="380" y="80" width="320" height="500" as="geometry"/>
</mxCell>

<!-- Infrastructure Section Container -->
<mxCell id="infra-section" value="📁 infrastructure/ (40+ files)"
  style="swimlane;startSize=30;strokeWidth=3;fillColor=#D5E8D4;strokeColor=#82B366;fontSize=14;fontStyle=1;shadow=1;swimlaneFillColor=#D5E8D4;"
  vertex="1" parent="1">
  <mxGeometry x="720" y="80" width="280" height="500" as="geometry"/>
</mxCell>
```

### Subsection Container Style

For nested directories within sections:

```xml
<!-- Subsection: aws modules -->
<mxCell id="aws-subsection" value="aws/ (8 modules)"
  style="swimlane;startSize=26;strokeWidth=2;fillColor=#FFFFFF;strokeColor=#6C8EBF;fontSize=12;fontStyle=1;swimlaneFillColor=#FFFFFF;"
  vertex="1" parent="kotlin-section">
  <mxGeometry x="10" y="40" width="300" height="140" as="geometry"/>
</mxCell>

<!-- Subsection: common modules -->
<mxCell id="common-subsection" value="common/ (7 modules)"
  style="swimlane;startSize=26;strokeWidth=2;fillColor=#E1D5E7;strokeColor=#9673A6;fontSize=12;fontStyle=1;swimlaneFillColor=#E1D5E7;"
  vertex="1" parent="kotlin-section">
  <mxGeometry x="10" y="190" width="300" height="120" as="geometry"/>
</mxCell>
```

### Module/Folder Item Style

Individual modules or folders within subsections:

```xml
<!-- Module item -->
<mxCell id="mod-dynamo" value="📦 dynamo-db"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontSize=11;"
  vertex="1" parent="aws-subsection">
  <mxGeometry y="26" width="300" height="22" as="geometry"/>
</mxCell>

<!-- File item -->
<mxCell id="file-gradle" value="📄 build.gradle.kts"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontSize=10;fontColor=#666666;"
  vertex="1" parent="kotlin-section">
  <mxGeometry y="470" width="300" height="20" as="geometry"/>
</mxCell>
```

### Icon Conventions

| Icon | Meaning | Usage |
|------|---------|-------|
| 📁 | Directory/Folder | Section headers, subsections |
| 📦 | Module/Package | Individual modules |
| 📄 | File | Individual files |
| ⚙️ | Configuration | Config files |
| 🔧 | Build Tool | Build scripts |

### Project Structure Legend

```xml
<!-- Legend -->
<mxCell id="legend" value="Legend"
  style="swimlane;fontStyle=1;childLayout=stackLayout;horizontal=1;startSize=26;horizontalStack=0;resizeParent=1;resizeParentMax=0;resizeLast=0;collapsible=0;marginBottom=0;fillColor=#F5F5F5;strokeColor=#666666;strokeWidth=2;fontSize=12;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="600" width="250" height="160" as="geometry"/>
</mxCell>
<mxCell id="leg1" value="■ Kotlin Modules"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontColor=#6C8EBF;fontSize=11;fontStyle=1;"
  vertex="1" parent="legend">
  <mxGeometry y="26" width="250" height="22" as="geometry"/>
</mxCell>
<mxCell id="leg2" value="■ Python Modules"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontColor=#D6B656;fontSize=11;fontStyle=1;"
  vertex="1" parent="legend">
  <mxGeometry y="48" width="250" height="22" as="geometry"/>
</mxCell>
<mxCell id="leg3" value="■ Infrastructure (Terraform)"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontColor=#82B366;fontSize=11;fontStyle=1;"
  vertex="1" parent="legend">
  <mxGeometry y="70" width="250" height="22" as="geometry"/>
</mxCell>
<mxCell id="leg4" value="■ Shared/Common"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontColor=#9673A6;fontSize=11;fontStyle=1;"
  vertex="1" parent="legend">
  <mxGeometry y="92" width="250" height="22" as="geometry"/>
</mxCell>
<mxCell id="leg5" value="■ Build/Config Files"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontColor=#666666;fontSize=11;fontStyle=1;"
  vertex="1" parent="legend">
  <mxGeometry y="114" width="250" height="22" as="geometry"/>
</mxCell>
<mxCell id="leg6" value="📁=Folder  📦=Module  📄=File"
  style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=10;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;whiteSpace=wrap;html=1;fontColor=#333333;fontSize=10;"
  vertex="1" parent="legend">
  <mxGeometry y="136" width="250" height="22" as="geometry"/>
</mxCell>
```

### Layout Guidelines

1. **Horizontal arrangement**: Place major sections (Kotlin, Python, Infrastructure) side by side
2. **Vertical stacking**: Stack subsections vertically within each main section
3. **Consistent widths**: Use consistent widths for sections (280-320px recommended)
4. **Spacing**: Leave 20-40px gaps between sections
5. **Module counts**: Always include module/file counts in section headers
6. **Page size**: Use larger page dimensions (1600x1000 or larger) for complex projects

### Complete Example Structure

```
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│ 📁 kotlin/      │  │ 📁 python/      │  │ 📁 infrastructure│
│ (26 modules)    │  │ (35 modules)    │  │ (40+ files)     │
├─────────────────┤  ├─────────────────┤  ├─────────────────┤
│ ┌─────────────┐ │  │ ┌─────────────┐ │  │ ┌─────────────┐ │
│ │ aws/        │ │  │ │ common/     │ │  │ │ app/        │ │
│ │ (8 modules) │ │  │ │ (11 mods)   │ │  │ │ (Terraform) │ │
│ └─────────────┘ │  │ └─────────────┘ │  │ └─────────────┘ │
│ ┌─────────────┐ │  │ ┌─────────────┐ │  │                 │
│ │ common/     │ │  │ │ function/   │ │  │ 📄 main.tf     │
│ │ (7 modules) │ │  │ │ (24 funcs)  │ │  │ 📄 lambda_*.tf │
│ └─────────────┘ │  │ └─────────────┘ │  │                 │
│ ┌─────────────┐ │  │ ┌─────────────┐ │  │                 │
│ │ function/   │ │  │ │ layer/      │ │  │                 │
│ │ (11 funcs)  │ │  │ │ (6 layers)  │ │  │                 │
│ └─────────────┘ │  │ └─────────────┘ │  │                 │
└─────────────────┘  └─────────────────┘  └─────────────────┘

┌──────────────────────────────────────┐
│ Legend                               │
│ ■ Kotlin  ■ Python  ■ Infrastructure │
│ 📁=Folder  📦=Module  📄=File        │
└──────────────────────────────────────┘
```

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

### Arrow Color Standards (Protocol-Based)

Arrow colors represent the **type of connection/protocol**, making it easy to identify data flow types at a glance.

#### Primary Connection Colors

| Connection Type | Color | Hex Code | Use Case |
|-----------------|-------|----------|----------|
| **HTTP/HTTPS/API** | Dark Gray | `#232F3E` | REST APIs, web requests, general traffic |
| **Database (SQL)** | Purple | `#C925D1` | PostgreSQL, MySQL, RDS, Aurora connections |
| **Cache** | Red | `#DC382D` | Redis, Memcached, ElastiCache |
| **Message Queue** | Pink | `#E7157B` | SQS, SNS, RabbitMQ, Kafka |
| **Storage** | Green | `#7AA116` | S3, Blob Storage, file systems |
| **Authentication** | Dark Red | `#DD344C` | OAuth, IAM, identity providers |
| **Monitoring/Logs** | Pink | `#E7157B` | CloudWatch, metrics, telemetry |

#### Secondary Connection Colors

| Connection Type | Color | Hex Code | Use Case |
|-----------------|-------|----------|----------|
| **gRPC** | Blue | `#4285F4` | gRPC service-to-service calls |
| **GraphQL** | Pink | `#E535AB` | GraphQL API endpoints |
| **WebSocket** | Teal | `#00BCD4` | Real-time bidirectional connections |
| **FTP/SFTP** | Brown | `#795548` | File transfer protocols |
| **SMTP/Email** | Orange | `#FF9800` | Email services |

#### Sync vs Async Indicators

| Pattern | Style | Use Case |
|---------|-------|----------|
| **Synchronous** | Solid line | HTTP, SQL, direct calls |
| **Asynchronous** | Dashed line (`dashed=1`) | Message queues, events, webhooks |
| **Polling** | Dashed + async arrow | Consumer polling from queue |

#### Color Application Examples

```xml
<!-- HTTP/API Request (gray) -->
<mxCell style="edgeStyle=none;rounded=0;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=classic;endFill=1;flowAnimation=1;shadow=1;textShadow=1;"
  edge="1" parent="1" source="users" target="alb">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Database Connection (purple) -->
<mxCell style="edgeStyle=none;rounded=0;html=1;strokeColor=#C925D1;strokeWidth=2;startArrow=classic;startFill=1;endArrow=classic;endFill=1;shadow=1;textShadow=1;"
  edge="1" parent="1" source="service" target="rds">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Cache Connection (red) -->
<mxCell style="edgeStyle=none;rounded=0;html=1;strokeColor=#DC382D;strokeWidth=2;startArrow=classic;startFill=1;endArrow=classic;endFill=1;shadow=1;textShadow=1;"
  edge="1" parent="1" source="service" target="redis">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Message Queue - Publish (pink, solid) -->
<mxCell style="edgeStyle=none;rounded=0;html=1;strokeColor=#E7157B;strokeWidth=2;endArrow=classic;endFill=1;flowAnimation=1;shadow=1;textShadow=1;"
  edge="1" parent="1" source="producer" target="sqs">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Message Queue - Consume (pink, dashed) -->
<mxCell style="edgeStyle=none;rounded=0;html=1;strokeColor=#E7157B;strokeWidth=2;dashed=1;dashPattern=8 8;endArrow=async;endFill=1;shadow=1;textShadow=1;"
  edge="1" parent="1" source="consumer" target="sqs">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Storage (green) -->
<mxCell style="edgeStyle=none;rounded=0;html=1;strokeColor=#7AA116;strokeWidth=2;startArrow=classic;startFill=1;endArrow=classic;endFill=1;shadow=1;textShadow=1;"
  edge="1" parent="1" source="service" target="s3">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

#### When to Use Neutral Gray

Use neutral gray (`#232F3E`) for ALL arrows when:
- The diagram focuses on **architecture structure** rather than data flow types
- You want a **cleaner, less colorful** appearance
- The connection types are obvious from context or labels

#### Arrow Color Quick Reference

```
#232F3E (gray)   ─────▶  HTTP/HTTPS, API, general
#C925D1 (purple) ◀────▶  Database (SQL read/write)
#DC382D (red)    ◀────▶  Cache (Redis, Memcached)
#E7157B (pink)   ─────▶  Message queue publish
#E7157B (pink)   - - -▷  Message queue consume (dashed)
#7AA116 (green)  ◀────▶  Storage (S3, files)
#DD344C (red)    ─ ─ ─▶  Auth/security (dashed)
```

---

## CRITICAL: Mandatory Arrow Legend Requirements

**Every diagram that uses meaningful arrow styles MUST include a legend section defining what each arrow color, style, and type represents.**

### When Arrow Legend is Required

A legend is **MANDATORY** when:
- Using **protocol-based colors** (different colors for HTTP, database, cache, queue, etc.)
- Using **ER relationship arrows** (ERone, ERmany, ERzeroToOne, etc.)
- Using **internal vs external request differentiation** (solid vs dashed, different colors)
- Using **sync vs async indicators** (solid vs dashed lines)
- Using **different arrow types** with semantic meaning (classic, open, async, block)
- Using **any custom arrow color or style** not standard gray

### Legend Position and Structure

**Position:** Bottom-left or bottom-right corner of the diagram, outside main content area.

**CRITICAL: Root Level Placement**
- Arrow Legend MUST be at root level: `parent="1"`
- Arrow Legend should NEVER be placed inside a group container (AWS Cloud, VPC, System Boundary, etc.)
- ALL legend elements (container box, line samples, text labels) must have `parent="1"`
- This ensures the legend remains independent and doesn't move with diagram groups

**Required Elements:**
1. **Legend container** - Gray background box with title "Legend" (`parent="1"`)
2. **Arrow samples** - Visual edge examples (`parent="1"`, use `edge="1"`)
3. **Text labels** - Clear description of what each arrow means (`parent="1"`)

### Arrow Legend Template (Infrastructure/Architecture Diagrams)

```xml
<!-- ================================================================ -->
<!-- ARROW LEGEND - Required for diagrams with meaningful arrow styles -->
<!-- ================================================================ -->
<mxCell id="arrow-legend" value="Arrow Legend"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;strokeWidth=2;verticalAlign=top;align=left;spacingLeft=10;spacingTop=5;fontStyle=1;fontSize=12;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="820" width="400" height="180" as="geometry"/>
</mxCell>

<!-- HTTP/API Arrow Sample -->
<mxCell id="legend-http-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=classic;endFill=1;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="860" as="sourcePoint"/>
    <mxPoint x="140" y="860" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-http-text" value="HTTP/HTTPS Request"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="850" width="130" height="20" as="geometry"/>
</mxCell>

<!-- Database Arrow Sample -->
<mxCell id="legend-db-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#C925D1;strokeWidth=2;startArrow=classic;startFill=1;endArrow=classic;endFill=1;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="890" as="sourcePoint"/>
    <mxPoint x="140" y="890" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-db-text" value="Database Read/Write (SQL)"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="880" width="150" height="20" as="geometry"/>
</mxCell>

<!-- Cache Arrow Sample -->
<mxCell id="legend-cache-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#DC382D;strokeWidth=2;startArrow=classic;startFill=1;endArrow=classic;endFill=1;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="920" as="sourcePoint"/>
    <mxPoint x="140" y="920" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-cache-text" value="Cache Read/Write (Redis)"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="910" width="150" height="20" as="geometry"/>
</mxCell>

<!-- Message Queue Publish Arrow Sample -->
<mxCell id="legend-mq-pub-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#E7157B;strokeWidth=2;endArrow=classic;endFill=1;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="950" as="sourcePoint"/>
    <mxPoint x="140" y="950" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-mq-pub-text" value="Queue Publish (async)"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="940" width="130" height="20" as="geometry"/>
</mxCell>

<!-- Message Queue Consume Arrow Sample -->
<mxCell id="legend-mq-con-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#E7157B;strokeWidth=2;dashed=1;dashPattern=8 8;endArrow=async;endFill=1;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="980" as="sourcePoint"/>
    <mxPoint x="140" y="980" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-mq-con-text" value="Queue Consume (polling)"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="970" width="140" height="20" as="geometry"/>
</mxCell>
```

### Arrow Legend Template (ERD Diagrams)

```xml
<!-- ================================================================ -->
<!-- ARROW LEGEND - Required for ERD diagrams with relationship arrows -->
<!-- ================================================================ -->
<mxCell id="erd-legend" value="Relationship Legend"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;strokeWidth=2;verticalAlign=top;align=left;spacingLeft=10;spacingTop=5;fontStyle=1;fontSize=12;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="1100" width="350" height="160" as="geometry"/>
</mxCell>

<!-- One-to-Many -->
<mxCell id="legend-one-many-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERmany;endFill=0;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="1140" as="sourcePoint"/>
    <mxPoint x="140" y="1140" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-one-many-text" value="One-to-Many (1:N)"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="1130" width="120" height="20" as="geometry"/>
</mxCell>

<!-- One-to-One -->
<mxCell id="legend-one-one-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERone;endFill=0;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="1170" as="sourcePoint"/>
    <mxPoint x="140" y="1170" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-one-one-text" value="One-to-One (1:1)"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="1160" width="120" height="20" as="geometry"/>
</mxCell>

<!-- Many-to-Many -->
<mxCell id="legend-many-many-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERmany;startFill=0;endArrow=ERmany;endFill=0;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="1200" as="sourcePoint"/>
    <mxPoint x="140" y="1200" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-many-many-text" value="Many-to-Many (M:N)"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="1190" width="130" height="20" as="geometry"/>
</mxCell>

<!-- Cross-database Sync -->
<mxCell id="legend-sync-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#9673a6;strokeWidth=2;dashed=1;dashPattern=8 8;endArrow=classic;endFill=1;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="1230" as="sourcePoint"/>
    <mxPoint x="140" y="1230" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-sync-text" value="Cross-DB Sync (async)"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="1220" width="140" height="20" as="geometry"/>
</mxCell>
```

### Arrow Legend Template (Internal vs External Traffic)

```xml
<!-- ================================================================ -->
<!-- ARROW LEGEND - Required for diagrams differentiating traffic types -->
<!-- ================================================================ -->
<mxCell id="traffic-legend" value="Traffic Legend"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;strokeWidth=2;verticalAlign=top;align=left;spacingLeft=10;spacingTop=5;fontStyle=1;fontSize=12;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="700" width="320" height="140" as="geometry"/>
</mxCell>

<!-- External Request (Internet) -->
<mxCell id="legend-ext-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#232F3E;strokeWidth=2;endArrow=classic;endFill=1;flowAnimation=1;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="740" as="sourcePoint"/>
    <mxPoint x="140" y="740" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-ext-text" value="External Request (Internet)"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="730" width="150" height="20" as="geometry"/>
</mxCell>

<!-- Internal Service-to-Service -->
<mxCell id="legend-int-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#4285F4;strokeWidth=2;endArrow=classic;endFill=1;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="770" as="sourcePoint"/>
    <mxPoint x="140" y="770" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-int-text" value="Internal Service-to-Service"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="760" width="150" height="20" as="geometry"/>
</mxCell>

<!-- VPN / Private Link -->
<mxCell id="legend-vpn-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#00BCD4;strokeWidth=2;dashed=1;dashPattern=8 8;endArrow=classic;endFill=1;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="800" as="sourcePoint"/>
    <mxPoint x="140" y="800" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-vpn-text" value="VPN / Private Link"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="790" width="120" height="20" as="geometry"/>
</mxCell>

<!-- Management / Monitoring -->
<mxCell id="legend-mgmt-line" value=""
  style="edgeStyle=none;rounded=0;html=1;strokeColor=#666666;strokeWidth=1;dashed=1;dashPattern=4 4;endArrow=open;endFill=0;shadow=1;"
  edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="60" y="830" as="sourcePoint"/>
    <mxPoint x="140" y="830" as="targetPoint"/>
  </mxGeometry>
</mxCell>
<mxCell id="legend-mgmt-text" value="Management / Monitoring"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;shadow=0;"
  vertex="1" parent="1">
  <mxGeometry x="150" y="820" width="150" height="20" as="geometry"/>
</mxCell>
```

### Minimal Legend (When Using Few Arrow Types)

For simple diagrams with only 2-3 arrow types, use a compact horizontal legend:

```xml
<!-- Compact horizontal legend -->
<mxCell id="compact-legend" value=""
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;strokeWidth=1;shadow=1;"
  vertex="1" parent="1">
  <mxGeometry x="40" y="700" width="500" height="40" as="geometry"/>
</mxCell>

<!-- Legend entries inline -->
<mxCell id="legend-sync" value="─────▶ Sync Request"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;fontColor=#232F3E;"
  vertex="1" parent="1">
  <mxGeometry x="50" y="710" width="120" height="20" as="geometry"/>
</mxCell>
<mxCell id="legend-async" value="- - -▷ Async Message"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;fontColor=#E7157B;"
  vertex="1" parent="1">
  <mxGeometry x="180" y="710" width="130" height="20" as="geometry"/>
</mxCell>
<mxCell id="legend-bidir" value="◀────▶ Read/Write"
  style="text;html=1;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;fontSize=10;fontColor=#C925D1;"
  vertex="1" parent="1">
  <mxGeometry x="320" y="710" width="110" height="20" as="geometry"/>
</mxCell>
```

### Legend Checklist

Before finalizing any diagram, verify:

| Check | Description |
|-------|-------------|
| ☐ | Legend container exists with gray background |
| ☐ | All arrow colors used in diagram are explained |
| ☐ | All arrow types (solid, dashed, bidirectional) are explained |
| ☐ | All ER relationship types are explained (if ERD) |
| ☐ | Legend is positioned outside main diagram content |
| ☐ | Text labels clearly describe arrow meaning |
| ☐ | Arrow samples match exact styles used in diagram |

---

### Edge Routing Styles

| Style | Description | Use Case |
|-------|-------------|----------|
| `orthogonalEdgeStyle` | Right-angle turns only | Clean architecture diagrams |
| `elbowEdgeStyle` | Single elbow bend | Simple L-shaped connections |
| `entityRelationEdgeStyle` | ER diagram style | Database relationships |
| `none` | Direct straight line | Point-to-point connections, dense layouts |

### Choosing Edge Style by Layout

**Use `edgeStyle=none` (straight) for complex architecture diagrams:**

When diagrams have multiple service icons of similar size (cloud architecture, microservices, etc.), use straight lines for ALL connections. This provides cleaner visuals and avoids routing complexity.

```
Complex Architecture (use straight for ALL):
    ┌───┐           ┌───┐           ┌───┐
    │ A │─────────▶ │ B │─────────▶ │ C │
    └───┘           └───┘     ╲     └───┘
       ╲              │        ╲
        ╲             ▼         ╲
         ╲          ┌───┐        ╲▶┌───┐
          ╲────────▶│ D │─────────▶│ E │
                    └───┘          └───┘
```

**When to use straight lines (`edgeStyle=none`):**
- **Complex architecture diagrams** - Multiple service icons (AWS, Azure, GCP, K8s)
- **Same-size shapes** - Icons of similar dimensions
- Dense shape clusters where orthogonal routing creates crossings

**When to use orthogonal (`edgeStyle=orthogonalEdgeStyle`):**
- **Simple diagrams** - Few shapes with clear row/column alignment
- **Flowcharts** - Sequential process flows
- **ER diagrams** - Database relationships (use `entityRelationEdgeStyle`)

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

### Color Palettes by Provider

**AWS Colors:**
| Category | Fill Color |
|----------|------------|
| Compute (Orange) | `#ED7100` |
| Storage (Green) | `#7AA116` |
| Database (Purple) | `#C925D1` |
| Networking (Purple) | `#8C4FFF` |
| Security (Red) | `#DD344C` |
| Analytics (Blue) | `#006FB7` |

**Azure Colors:**
| Category | Fill Color |
|----------|------------|
| Compute | `#0078D4` |
| Storage | `#0078D4` |
| Database | `#0078D4` |
| Networking | `#0078D4` |
| Security | `#5C2D91` |

**GCP Colors:**
| Category | Fill Color |
|----------|------------|
| Compute (Blue) | `#4285F4` |
| Storage (Blue) | `#4285F4` |
| Database (Blue) | `#4285F4` |
| Networking (Purple) | `#9C27B0` |
| Security (Green) | `#34A853` |
| AI/ML (Orange) | `#EA4335` |

**Kubernetes Colors:**
| Category | Fill Color |
|----------|------------|
| Primary Blue | `#326CE5` |
| Pod Green | `#4CAF50` |
| Service Yellow | `#FFC107` |

**General Architecture Colors:**
| Purpose | Fill Color | Stroke Color |
|---------|------------|--------------|
| Public/External | `#d5e8d4` | `#82b366` |
| Private/Internal | `#dae8fc` | `#6c8ebf` |
| Security Boundary | `#f8cecc` | `#b85450` |
| Data/Storage | `#e1d5e7` | `#9673a6` |
| Integration/Messaging | `#fff2cc` | `#d6b656` |
| User/Client | `#f5f5f5` | `#666666` |

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
1. **Always use official shape references** - Never use generic rectangles for cloud/infrastructure services
2. **Match provider/domain to request:**
    - AWS diagram → `mxgraph.aws4.*` shapes
    - Azure diagram → `mxgraph.azure.*` shapes
    - GCP diagram → `mxgraph.gcp2.*` shapes
    - Kubernetes → `mxgraph.kubernetes.*` shapes
    - Network diagram → `mxgraph.networks.*` or `mxgraph.cisco.*` shapes
    - Software architecture → UML or custom component shapes
3. **Include proper groups** - Use VPC/Subnet/Namespace/Zone groups to show boundaries
4. **CRITICAL: Set correct parent attributes** - Children must have `parent="<container-id>"` to move with their container
5. **Use relative coordinates for nested elements** - Child coordinates are relative to parent's top-left
6. **XML order determines z-order** - Containers first → Children after → Edges last
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
    - `ERone`/`ERmany` - Database relationships (ER diagrams)
    - `none` - Associations without direction
13. **Add edge labels for protocols** - Label connections with protocol names (HTTPS, gRPC, TCP:5432, etc.)
14. **Use dashed lines for management relationships** - Set `dashed=1` for control plane, monitoring, or optional paths
15. **CRITICAL: Include Arrow Legend when using meaningful arrow styles** - Every diagram with protocol-based colors, ER relationships, or internal/external traffic differentiation MUST include a legend explaining arrow meanings. **Arrow Legend MUST be at root level (`parent="1"`), NEVER inside a group container.**

### Frames and Containers
15. **Use hierarchical stroke widths for groups:**
    - Top-level (Cloud/Data Center): `strokeWidth=4`
    - Sub-groups (Region, VPC, Namespace): `strokeWidth=3`
    - Leaf groups (Subnets, Pods): `strokeWidth=2`
16. **Group shapes must have square corners** - Always use `rounded=0` for container/group shapes
17. **Apply consistent frame colors by purpose:**
    - Green (`#d5e8d4`) - Public/External
    - Blue (`#dae8fc`) - Private/Internal
    - Red (`#f8cecc`) - Security Boundary
    - Yellow (`#fff2cc`) - Integration/DMZ
    - Purple (`#e1d5e7`) - Data/Storage
18. **Position frame labels consistently** - Use `verticalAlign=top;align=left` for frame titles

### Edge Routing and Shape Positioning
19. **CRITICAL: Edges must NEVER cross shapes** - Position shapes so connected items are adjacent
20. **Position connected shapes adjacently** - Shapes that connect should be neighbors
21. **Use row-based layout** - Organize shapes in rows where each row represents a logical tier
22. **Stagger non-connected shapes** - Avoid grid alignment that forces diagonal crossings
23. **Use entry/exit points** - Control connection angles with `exitX`, `exitY`, `entryX`, `entryY`
24. **Maintain edge spacing** - Keep 20px between parallel edges, 10px from shapes

### Domain-Specific Guidelines

**Cloud Architecture:**
- Show region/zone boundaries
- Group resources by VPC/VNet/VPC
- Indicate public vs private subnets
- Show internet gateways and NAT gateways
- **Include Arrow Legend** for protocol colors (HTTP, DB, Cache, Queue)

**Kubernetes:**
- Show namespace boundaries
- Group pods by deployment/service
- Indicate ingress paths
- Show config/secrets relationships

**Network Diagrams:**
- Use consistent IP addressing labels
- Show firewall rules as text labels
- Indicate VPN/tunnel connections with dashed lines
- Group by network zones (DMZ, Internal, External)
- **Include Arrow Legend** for internal vs external traffic, VPN, management flows

**Software Architecture:**
- Use layers (Presentation, Business, Data)
- Show synchronous vs asynchronous communication
- Indicate databases with cylinder shapes
- Group microservices by domain
- **Include Arrow Legend** when differentiating sync vs async, protocols

**Integration Architecture:**
- Show message flow direction
- Indicate queue/topic names
- Use different colors for sync vs async
- Show retry/error paths with dashed lines
- **Include Arrow Legend** explaining sync/async colors and patterns

**Database ERD:**
- Use `swimlane` shape for tables with column details
- Use `entityRelationEdgeStyle` with ER arrows (ERone, ERmany, ERzeroToOne)
- Minimum 200px horizontal spacing between tables
- Minimum 80px vertical spacing between rows
- Highlight central/core table with yellow (`#FFF2CC`)
- Group tables by domain (User, Product, Transaction, etc.)
- Place NoSQL/Cache/Search databases in separate column
- Route cross-database sync arrows around perimeter (dashed lines)
- Include legend showing database types and relationship notations

**C4 Model:**
- Create separate diagram pages for each level (Context, Container, Component)
- Use correct colors: Person (`#08427B`), System (`#438DD5`), External (`#999999`), Component (`#85BBF0`)
- Main system uses `strokeWidth=3`, supporting elements use `strokeWidth=2`
- System/Container boundaries use dashed lines with `strokeWidth=4` or `strokeWidth=3`
- **CRITICAL: flowAnimation only on unidirectional arrows**
  - Single arrow (→): Add `flowAnimation=1`
  - Bidirectional (↔): NO flowAnimation
  - Async/Subscribe (▷): NO flowAnimation
- Label all edges with protocol/technology (REST, gRPC, SQL, etc.)
- Include legend explaining colors and arrow types
- Use HTML formatting in labels: `<b>Name</b><br>[Type]<br><br><font style='font-size:10px'>Description</font>`

### General Guidelines
25. **Use correct colors** - Follow the provider's color palette for each service category
26. **Label all components** - Include service names in the `value` attribute
27. **Position logically** - Arrange components following left-to-right or top-to-bottom flow
28. **Be consistent** - Use same icon size, spacing, and style throughout the diagram
29. **ALWAYS include Arrow Legend at root level** - When using meaningful arrow colors/styles (protocols, ER relationships, internal/external traffic), include a legend section explaining each arrow type. Legend MUST have `parent="1"`, never inside groups.
