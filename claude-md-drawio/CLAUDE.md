# CLAUDE.md - DrawIO Architecture Diagram Guidelines

## Overview

Comprehensive guide for creating `.drawio` architecture diagrams covering:
- **Cloud Platforms**: AWS, Azure, Google Cloud (GCP)
- **Container Orchestration**: Kubernetes (K8s), Docker
- **Network & Infrastructure**: Firewalls, Load Balancers, DNS, VPN
- **Software Architecture**: Microservices, APIs, Databases
- **Integration Systems**: Message Queues, ESB, API Gateways
- **IT Development**: CI/CD, DevOps, Monitoring
- **Database ERD**: Entity-Relationship Diagrams with multi-database support (SQL, NoSQL, Cache, Search)

DrawIO uses XML format with specific style attributes to reference official shape libraries for each domain.

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

**Edge Style by Connection Direction:**

| Connection Direction | Edge Style | Reason |
|---------------------|------------|--------|
| Horizontal (left ↔ right) | `entityRelationEdgeStyle` | Direct straight line connection |
| Vertical (top ↔ bottom) | `orthogonalEdgeStyle;rounded=1` | Proper right-angle routing for waypoints |
| Diagonal/Mixed | `orthogonalEdgeStyle;rounded=1` | Clean orthogonal routing with waypoints |

```xml
<!-- Horizontal connection: entityRelationEdgeStyle (left-right) -->
<mxCell id="rel-users-orders"
  style="edgeStyle=entityRelationEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERmany;endFill=0;shadow=1;"
  edge="1" parent="1" source="tbl-users" target="tbl-orders">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Vertical connection: orthogonalEdgeStyle (top-bottom) -->
<mxCell id="rel-users-properties"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERmany;endFill=0;shadow=1;exitX=0.5;exitY=1;entryX=0.5;entryY=0;"
  edge="1" parent="1" source="tbl-users" target="tbl-properties">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Horizontal optional relationship -->
<mxCell id="rel-transactions-contracts"
  style="edgeStyle=entityRelationEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;strokeColor=#336791;strokeWidth=2;startArrow=ERone;startFill=0;endArrow=ERzeroToOne;endFill=0;shadow=1;"
  edge="1" parent="1" source="tbl-transactions" target="tbl-contracts">
  <mxGeometry relative="1" as="geometry"/>
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

**Software Architecture:**
- Use layers (Presentation, Business, Data)
- Show synchronous vs asynchronous communication
- Indicate databases with cylinder shapes
- Group microservices by domain

**Integration Architecture:**
- Show message flow direction
- Indicate queue/topic names
- Use different colors for sync vs async
- Show retry/error paths with dashed lines

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

### General Guidelines
25. **Use correct colors** - Follow the provider's color palette for each service category
26. **Label all components** - Include service names in the `value` attribute
27. **Position logically** - Arrange components following left-to-right or top-to-bottom flow
28. **Be consistent** - Use same icon size, spacing, and style throughout the diagram
