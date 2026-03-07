# Cloud Architecture Diagrams

Sample cloud architecture diagrams demonstrating AWS, Azure, and Kubernetes deployments.

## Diagrams

| Diagram | Description | Preview |
|---------|-------------|---------|
| **AWS ECS** | VPC, subnets, Fargate, RDS, Redis, SQS | ![Preview](sample-architecture-aws-ecs.png) |
| **Azure WebApp** | App Service, Functions, SQL, Cosmos DB, Redis | ![Preview](sample-architecture-azure-webapp.png) |
| **AWS + K8s Insurance Portal** | Hybrid AWS cloud + On-prem Kubernetes | ![Preview](sample-architecture-aws-portal-k8s-insurance.png) |
| **On-Prem K8s CoBroker Portal** | Full Kubernetes on-premises with data layer | ![Preview](sample-architecture-insurance-cobroker-portal-k8s-onprem.png) |

## Files

| File | Type |
|------|------|
| [sample-architecture-aws-ecs.drawio](./sample-architecture-aws-ecs.drawio) | DrawIO Source |
| [sample-architecture-aws-ecs.png](./sample-architecture-aws-ecs.png) | PNG Export |
| [sample-architecture-azure-webapp.drawio](./sample-architecture-azure-webapp.drawio) | DrawIO Source |
| [sample-architecture-azure-webapp.png](./sample-architecture-azure-webapp.png) | PNG Export |
| [sample-architecture-aws-portal-k8s-insurance.drawio](./sample-architecture-aws-portal-k8s-insurance.drawio) | DrawIO Source |
| [sample-architecture-aws-portal-k8s-insurance.png](./sample-architecture-aws-portal-k8s-insurance.png) | PNG Export |
| [sample-architecture-insurance-cobroker-portal-k8s-onprem.drawio](./sample-architecture-insurance-cobroker-portal-k8s-onprem.drawio) | DrawIO Source |
| [sample-architecture-insurance-cobroker-portal-k8s-onprem.png](./sample-architecture-insurance-cobroker-portal-k8s-onprem.png) | PNG Export |

## Standards Applied

- Protocol-based arrow colors (HTTP: gray, DB: purple, Cache: red, Queue: pink)
- Hierarchical stroke widths (Cloud: 4pt, Region/VPC: 3pt, Subnet: 2pt)
- Shadow on all elements
- AWS `resourceIcon` format for service icons
- Azure `img/lib/azure2/` SVG icons
