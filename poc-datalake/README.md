# Data Lake Architecture Diagrams

Sample data lake architecture diagrams demonstrating AWS-based data lake implementations with ingestion, storage, processing, and analytics layers.

## Diagrams

| Diagram | Description | Preview |
|---------|-------------|---------|
| **AWS Data Lake** | S3 zones, Glue ETL, Athena, Redshift, QuickSight | ![Preview](sample-datalake-aws.png) |

## Files

| File | Type |
|------|------|
| [sample-datalake-aws.drawio](./sample-datalake-aws.drawio) | DrawIO Source |
| [sample-datalake-aws.png](./sample-datalake-aws.png) | PNG Export |

## Architecture Overview

### Data Lake Zones

| Zone | Purpose | S3 Tier |
|------|---------|---------|
| **Raw Zone** | Landing zone for raw data as-is | S3 Standard |
| **Processed Zone** | Cleaned, validated, transformed data | S3 Standard |
| **Curated Zone** | Business-ready, aggregated datasets | S3 Standard |
| **Archive Zone** | Historical data for compliance | S3 Glacier |

### Key Components

| Layer | Services |
|-------|----------|
| **Ingestion** | Kinesis Data Streams, Kinesis Firehose, AWS DMS, Lambda |
| **Storage** | S3 (multi-zone), Lake Formation |
| **Processing** | AWS Glue, EMR, Athena |
| **Analytics** | Redshift, QuickSight |
| **Governance** | Lake Formation, IAM, CloudTrail |

## Standards Applied

- Protocol-based arrow colors (Streaming: orange, ETL: purple, Query: blue, Storage: green)
- Hierarchical stroke widths (Cloud: 4pt, Account: 3pt, VPC/Zone: 2pt)
- Shadow on all elements
- AWS `resourceIcon` format for service icons
- Flow animation for unidirectional data flows
