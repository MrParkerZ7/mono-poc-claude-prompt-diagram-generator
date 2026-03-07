# C4 Model Diagrams

Sample C4 Model architecture diagrams showing Context, Container, and Component levels.

## Diagrams

| Diagram | Description | Preview |
|---------|-------------|---------|
| **Core Banking** | ITMX core banking system with multiple integration points | ![Preview](sample-c4-model-core-banking.png) |

## Files

| File | Type |
|------|------|
| [sample-c4-model-core-banking.drawio](./sample-c4-model-core-banking.drawio) | DrawIO Source |
| [sample-c4-model-core-banking.png](./sample-c4-model-core-banking.png) | PNG Export |

## C4 Model Levels

The diagram includes multiple pages:
- **Context** - System landscape, users, external systems
- **Container** - Applications, databases, services
- **Component** - Internal structure of containers

## Standards Applied

- C4 color scheme (Person: #08427B, System: #438DD5, External: #999999)
- Main system uses strokeWidth=3, supporting elements use strokeWidth=2
- Dashed boundaries for system/container scope
- flowAnimation only on unidirectional arrows
- Labels with protocol/technology (REST, gRPC, SQL)
