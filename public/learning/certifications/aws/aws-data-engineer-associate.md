---
title: "AWS Certified Data Engineer Associate Study Guide"
tags: [learning, certification, aws, data]
created: 2025-12-18
---

# AWS Certified Data Engineer Associate Study Guide

Study guide for the AWS Data Engineer Associate certification.

## Exam Overview

| Detail | Value |
|--------|-------|
| Duration | 170 minutes |
| Questions | 65 |
| Passing Score | 720/1000 |
| Cost | $150 |

## Exam Domains

| Domain | Weight | Key Topics |
|--------|--------|------------|
| 1. Data Ingestion & Transformation | 34% | Kinesis, Glue, DMS, batch/streaming |
| 2. Data Store Management | 26% | S3, Redshift, DynamoDB, RDS |
| 3. Data Operations & Support | 22% | Pipelines, monitoring, troubleshooting |
| 4. Data Security & Governance | 18% | Encryption, Lake Formation, compliance |

## Key Services to Master

### Data Ingestion (Highest Priority)

**Amazon Kinesis**
- Kinesis Data Streams - real-time streaming
- Kinesis Data Firehose - managed delivery to destinations
- Kinesis Data Analytics - SQL/Flink on streaming data

**AWS Glue**
- Glue ETL jobs (Python/Scala)
- Glue crawlers and Data Catalog
- Glue Studio (visual ETL)
- Glue DataBrew (data preparation)
- Glue workflows and triggers

**AWS DMS**
- Database Migration Service
- CDC (Change Data Capture) patterns
- Homogeneous vs heterogeneous migrations

### Data Storage

**Amazon S3**
- Data lake architecture
- Storage classes and lifecycle
- S3 Select and Glacier Select

**Amazon Redshift**
- Cluster architecture
- Distribution styles and sort keys
- Redshift Spectrum for S3 queries
- Materialized views

**Amazon DynamoDB**
- Partition keys and sort keys
- GSI and LSI
- DynamoDB Streams
- Export to S3

### Data Processing

**Amazon EMR**
- Spark, Hive, Presto
- EMR on EKS
- EMR Serverless

**AWS Step Functions**
- Orchestrating data pipelines
- Error handling and retries

**Amazon Athena**
- Serverless SQL on S3
- Partitioning strategies
- CTAS and views

### Data Governance

**AWS Lake Formation**
- Data lake permissions
- Column-level security
- Data sharing

**AWS Glue Data Catalog**
- Metadata management
- Schema versioning

---

## Study Plan (7 Days Intensive)

### Day 1-2: Data Ingestion (34%)
- Kinesis Data Streams vs Firehose decision scenarios
- AWS Glue ETL jobs and crawlers
- DMS migration patterns
- Lambda for event-driven ingestion

### Day 3-4: Data Storage (26%)
- S3 data lake patterns
- Redshift architecture and optimization
- DynamoDB design patterns
- When to use which database

### Day 5: Data Operations (22%)
- Step Functions for orchestration
- CloudWatch monitoring for data pipelines
- Troubleshooting common issues

### Day 6: Security & Governance (18%)
- Lake Formation permissions
- Encryption at rest and in transit
- VPC endpoints for data services

### Day 7: Practice & Review
- Full practice exam
- Review weak areas
- Re-read key documentation

---

## Decision Frameworks

### Streaming vs Batch

| Use Case | Solution |
|----------|----------|
| Real-time analytics (<1 sec) | Kinesis Data Streams + Lambda |
| Near real-time (1-60 sec) | Kinesis Data Firehose |
| Batch processing (hourly+) | Glue ETL + S3 |
| Complex transformations | EMR or Glue |

### Storage Selection

| Use Case | Solution |
|----------|----------|
| Data lake storage | S3 |
| OLAP analytics | Redshift |
| Key-value, high throughput | DynamoDB |
| Relational, OLTP | RDS/Aurora |
| Graph data | Neptune |
| Time series | Timestream |

---

## Recommended Resources

### Courses
- AWS Skill Builder: Data Engineer Learning Plan
- Udemy: Stephane Maarek & Frank Kane Data Engineer course

### Hands-on Labs
- Build a data lake with S3 and Glue
- Create streaming pipeline with Kinesis
- Set up Redshift cluster with Spectrum

### Practice Exams
- AWS Skill Builder official practice exam
- Tutorials Dojo practice tests
