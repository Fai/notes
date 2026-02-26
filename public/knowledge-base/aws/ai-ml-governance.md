---
title: AI/ML Governance with SageMaker & Bedrock
tags: [aws, sagemaker, bedrock, governance, data-catalog, compliance]
created: 2025-12-18
---

# AI/ML Governance with SageMaker & Bedrock

Enterprise data governance for generative AI solutions using SageMaker Unified Studio and Amazon Bedrock.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│         Amazon SageMaker Unified Studio                      │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────────┐  ┌──────────────────┐  ┌────────────┐│
│  │  Data Sources    │  │  Amazon Bedrock  │  │ SageMaker  ││
│  │  - S3, Redshift  │  │  - LLMs          │  │ Catalog    ││
│  │  - RDS, Athena   │  │  - Knowledge Base│  │ (DataZone) ││
│  │                  │  │  - Guardrails    │  │ Governance ││
│  └──────────────────┘  └──────────────────┘  └────────────┘│
│  ┌──────────────────────────────────────────────────────────┤
│  │  Fine-Grained Access Control & Data Lineage              │
│  └──────────────────────────────────────────────────────────┤
└─────────────────────────────────────────────────────────────┘
```

## Bedrock Integration Features

### GenAI App Types

| Type | Description | Use Case |
|------|-------------|----------|
| Chat Agent Apps | Conversational AI with knowledge bases | Customer support |
| Flow Apps | Visual workflow builder | Document processing |

### Key Bedrock Features

1. **Knowledge Bases** - RAG with company data
2. **Guardrails** - Content filtering, PII redaction, topic blocking
3. **Agents** - AI that can call APIs and take actions
4. **Prompt Management** - Versioned, reusable prompts

### Model Selection Guide

| Use Case | Recommended Model | Reason |
|----------|------------------|--------|
| Customer Support | Claude 3 | Best for conversations, safety |
| Document Analysis | Claude 3 Sonnet | Excellent document understanding |
| Code Generation | Claude 3.5 Sonnet | Superior coding |
| Cost-Sensitive | Llama 3 | Open-source, lower cost |

## SageMaker Catalog (Data Governance)

Built on Amazon DataZone for enterprise data governance.

### Core Capabilities

1. **Data Cataloging** - Centralized metadata management
2. **Fine-Grained Access Control** - Row/column/cell-level security
3. **Data Lineage** - Track data flow end-to-end
4. **Data Quality** - Automated quality monitoring
5. **Business Glossary** - Standardized terminology
6. **Data Products** - Curated, versioned datasets

### Access Control Model

```
Permission Levels:
├── Row-level security: Filter data by user
├── Column-level security: Hide sensitive columns
├── Cell-level security: Mask specific values
└── Time-based access: Temporary permissions
```

**Example Policies:**
- Data Scientist: Aggregated data, no PII
- Business Analyst: Regional data only
- Compliance Officer: Full access, read-only
- ML Engineer: Training access, PII masked

### Data Lineage Tracking

```
Source Data (S3)
    ↓
Data Wrangler (Transformation)
    ↓
Feature Store
    ↓
SageMaker Training Job
    ↓
Deployed Model
    ↓
Predictions (Redshift)
```

**Benefits:**
- Impact analysis for changes
- Compliance audit trails
- Data quality debugging
- Unused data identification

### Data Quality Monitoring

**Automated Checks:**
- Completeness (missing values)
- Accuracy (validation rules)
- Consistency (cross-field validation)
- Timeliness (data freshness)
- Uniqueness (duplicate detection)

**Quality Dashboard:**
```
Dataset: customer_transactions
├── Overall Quality: 92/100
├── Completeness: 98/100 ✓
├── Accuracy: 95/100 ✓
├── Consistency: 88/100 ⚠️
└── Timeliness: 90/100 ✓
```

## Use Case Examples

### Customer Service Chatbot (Banking)

```
Customer Question
    ↓
Bedrock Claude Model
    ↓
Knowledge Base (Banking policies)
    ↓
Guardrails (PII redaction, compliance)
    ↓
Response
```

**Benefits:** 80% support cost reduction, 24/7 availability, compliance

### Document Intelligence (Insurance)

```
Claim Documents (PDF)
    ↓
Amazon Textract (OCR)
    ↓
Bedrock (Document Understanding)
    ↓
SageMaker ML (Fraud Detection)
    ↓
Automated Decision
```

**Benefits:** 70% faster processing, 95% accuracy

## Compliance Considerations

| Regulation | Requirements |
|------------|--------------|
| PDPA (Thailand) | Data privacy, consent management |
| GDPR | International data protection |
| HIPAA | Healthcare data security |
| Basel III | Banking risk management |

## Bedrock Pricing Reference

| Component | Cost |
|-----------|------|
| Claude 3 Haiku | ~$0.00025/1K input tokens |
| Claude 3 Sonnet | ~$0.003/1K input tokens |
| Knowledge Bases | $0.10/GB/month storage |
| Guardrails | $0.75/1K tokens |

**Example:** 100K conversations/month @ 500 tokens avg = ~$160/month

## Related Articles

- [[bedrock-knowledge-bases]] - RAG implementation
- [[bedrock-cost-control]] - Cost management
- [[security-checklist]] - Security best practices
