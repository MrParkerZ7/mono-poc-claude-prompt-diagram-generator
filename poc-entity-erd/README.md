# Entity-Relationship Diagrams (ERD)

Sample database ERD diagrams with multi-database support (SQL, NoSQL, Cache, Search).

## Diagrams

| Diagram | Description | Preview |
|---------|-------------|---------|
| **Property Agent Database** | Multi-database ERD with PostgreSQL, MongoDB, Redis, Elasticsearch | ![Preview](sample-entity-property-agent-database.png) |

## Files

| File | Type |
|------|------|
| [sample-entity-property-agent-database.drawio](./sample-entity-property-agent-database.drawio) | DrawIO Source |
| [sample-entity-property-agent-database.png](./sample-entity-property-agent-database.png) | PNG Export |

## Database Types Shown

| Database | Color | Use Case |
|----------|-------|----------|
| PostgreSQL | Blue (#336791) | Primary relational data |
| MongoDB | Green (#4DB33D) | Document store |
| Redis | Red (#DC382D) | Cache layer |
| Elasticsearch | Orange (#b85450) | Full-text search |

## Standards Applied

- Swimlane format for tables with column details
- ER arrows (ERone, ERmany) for relationships
- Horizontal-only connections (no vertical ER arrows)
- Database-specific colors for grouping
- Cross-database sync shown with dashed arrows
- HTML tables for column separation (PK/FK | Name | Type)
