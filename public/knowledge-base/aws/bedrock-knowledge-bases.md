---
title: "Bedrock Knowledge Bases"
title: "Bedrock Knowledge Bases"
tags: [aws, bedrock, ai-ml, rag, knowledge-base]
created: 2025-12-18
updated: 2025-12-18
---
title: "Bedrock Knowledge Bases"

# Amazon Bedrock Knowledge Bases Guide

Overview of Bedrock Knowledge Base types, costs, and implementation patterns.

## Knowledge Base Types

| Type | Storage Backend | Monthly Cost | Best For |
|------|-----------------|--------------|----------|
| **Vector (OpenSearch)** | OpenSearch Serverless | $700+ | Full-text + semantic search |
| **S3 Vectors** (Preview) | S3 | $0.13 - $5 | Cost-sensitive, small chunks |
| **SQL** | Redshift Serverless | $260 - $500 | Structured data, analytics |
| **Aurora Vector** | Aurora PostgreSQL | $44 - $100 | Frequent queries, relational |

## Cost Comparison

### By Deployment Size

| Size | OpenSearch | S3 Vectors | SQL (Redshift) |
|------|------------|------------|----------------|
| Small (1-5 bots) | $700 | $5 | $260 |
| Medium (5-20 bots) | $1,200 | $50 | $400 |
| Large (20+ bots) | $2,000+ | $200 | $500+ |

**Key Insight:** S3 Vectors can provide 50-94% cost savings for most workloads.

## Vector KB (OpenSearch Serverless)

### Configuration
```typescript
// CDK Example
const collection = new opensearchserverless.CfnCollection(this, 'KBCollection', {
  name: 'knowledge-base',
  type: 'VECTORSEARCH',
});
```

### Chunking Strategies
- `default` - Automatic chunking
- `fixed_size` - Fixed character count
- `hierarchical` - Parent-child chunks
- `semantic` - Meaning-based boundaries
- `none` - No chunking

### Embedding Models
- Titan Text Embeddings v2
- Cohere Multilingual v3

## SQL KB (Redshift Serverless)

### When to Use
- Structured data queries
- Analytics workloads
- Large datasets with relationships
- SQL-based question answering

### Configuration
```typescript
// Redshift Serverless
- Engine: Amazon Redshift Serverless
- Scaling: 8-64 RPU
- VPC: Private subnets
- Data API: Enabled
```

### Implementation Pattern
1. Create Redshift Serverless workgroup
2. Define schema and tables
3. Configure Bedrock KB with SQL data source
4. Set up IAM roles for access

## S3 Vectors (Preview)

### When to Use
- Cost-sensitive deployments
- Smaller document sets (<500 token chunks)
- Infrequent queries
- Development/testing

### Limitations
- Preview feature
- Best for smaller chunks
- May have higher latency than OpenSearch

## Best Practices

### Cost Optimization
1. Start with S3 Vectors for dev/test
2. Use OpenSearch only when needed
3. Monitor OCU usage in OpenSearch
4. Consider Aurora for frequent queries

### Performance
1. Choose appropriate chunking strategy
2. Use hybrid search (vector + keyword)
3. Enable caching where possible
4. Monitor query latency

### Security
1. Use VPC endpoints
2. Enable encryption at rest
3. Implement row-level security for SQL
4. Use IAM roles, not credentials

## Monitoring

### Key Metrics
- Query latency (p50, p95, p99)
- OCU utilization (OpenSearch)
- RPU usage (Redshift)
- Embedding costs
- Storage costs

### CloudWatch Alarms
```bash
# High latency alert
aws cloudwatch put-metric-alarm \
  --alarm-name kb-high-latency \
  --metric-name Latency \
  --namespace AWS/Bedrock \
  --threshold 5000 \
  --comparison-operator GreaterThanThreshold
```

## Related
- [Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/)
- [OpenSearch Serverless](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/serverless.html)
