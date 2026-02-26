# AWS Certified Generative AI Developer Professional Study Guide

Comprehensive study guide for the AWS Generative AI Developer Professional certification.

## Exam Overview

| Detail | Value |
|--------|-------|
| Duration | 170 minutes |
| Questions | 65 |
| Passing Score | 750/1000 |
| Cost | $300 |

## Exam Domains

| Domain | Weight | Focus |
|--------|--------|-------|
| 1. FM Integration, Data Management, Compliance | 31% | Bedrock, RAG, vector stores, prompt engineering |
| 2. Implementation and Integration | 26% | Agents, deployment, API integration |
| 3. AI Safety, Security, and Governance | 20% | Guardrails, data privacy, responsible AI |
| 4. Operational Efficiency and Optimization | 12% | Cost optimization, performance, monitoring |
| 5. Testing, Validation, and Troubleshooting | 11% | Evaluation systems, debugging |

---

## Domain 1: Foundation Model Integration (31%)

### Task 1.1: Design GenAI Solutions

**Key Skills:**
- Select appropriate FMs for business needs
- Develop prototypes using Amazon Bedrock
- Apply AWS Well-Architected Framework (GenAI Lens)

**Services to Know:**
- **Amazon Bedrock** - Unified API for FMs (Anthropic, AI21, Cohere, Meta, Stability AI, Amazon)
- **Amazon SQS/Kafka** - Event-driven integration patterns
- **AWS AppSync** - GraphQL API layer with Bedrock integration
- **AWS Lambda** - Serverless GenAI applications

### Task 1.2: Select and Configure FMs

**Key Skills:**
- Assess FMs based on performance benchmarks
- Design flexible architectures for model switching
- Implement failover mechanisms

### Task 1.3: Data Validation and Processing

**Key Skills:**
- Create data validation workflows
- Handle multimodal data (text, image, audio)
- Format inputs for model-specific requirements

### Task 1.4: Vector Store Solutions

**Key Skills:**
- Design vector database architectures
- Implement metadata frameworks with S3
- Maintain vector stores for accuracy

**Services to Know:**
- **Amazon OpenSearch Serverless** - Vector search
- **Amazon Aurora PostgreSQL** - pgvector extension
- **Amazon Bedrock Knowledge Bases** - Managed RAG

### Task 1.5: Retrieval Mechanisms (RAG)

**Key Skills:**
- Document chunking strategies
- Embedding selection and configuration
- Query handling and result ranking

**Key Concepts:**
- Chunking methods (fixed-size, semantic, recursive)
- Embedding models selection
- Hybrid search (vector + keyword)
- Reranking strategies

### Task 1.6: Prompt Engineering

**Key Skills:**
- Create model instruction frameworks
- Build conversational AI with context
- Implement prompt governance
- Design prompt chains and conditional branching

---

## Domain 2: Implementation and Integration (26%)

### Task 2.1: Agentic AI Solutions
- Amazon Bedrock Agents
- Tool integration patterns
- Multi-step reasoning

### Task 2.2: Model Deployment
- SageMaker deployment options (real-time, batch, async, serverless)
- Model optimization techniques

### Task 2.3: Enterprise Integration
- API Gateway integration
- WebSocket APIs for streaming
- Cross-region inference

### Task 2.4: FM API Integration
- Bedrock Runtime API
- Converse API
- Streaming responses

### Task 2.5: Development Tools
- Amazon Q Developer
- Code generation best practices

---

## Domain 3: AI Safety and Security (20%)

### Task 3.1: Input/Output Safety
- Amazon Bedrock Guardrails
- Content filtering
- PII detection and redaction

### Task 3.2: Data Security
- IAM policies for Bedrock
- Temporary credentials (STS)
- Cognito integration
- IAM Identity Center (SSO)

### Task 3.3: Governance and Compliance
- Model invocation logging
- CloudTrail integration
- Knowledge base logging

### Task 3.4: Responsible AI
- Bias detection (SageMaker Clarify)
- Model transparency
- Ethical considerations

---

## Domain 4: Operational Efficiency (12%)

### Task 4.1: Cost Optimization
- Token usage optimization
- Model selection for cost/performance
- Provisioned throughput vs on-demand

### Task 4.2: Performance Optimization
- Latency reduction strategies
- Caching strategies
- Batch processing

### Task 4.3: Monitoring
- CloudWatch metrics for Bedrock
- Guardrails monitoring
- Custom dashboards (Grafana)

---

## Domain 5: Testing and Troubleshooting (11%)

### Task 5.1: Evaluation Systems
- Amazon Bedrock Model Evaluation
- A/B testing with SageMaker
- Custom evaluation metrics

### Task 5.2: Troubleshooting
- Common failure patterns
- Debugging RAG pipelines
- Performance bottlenecks

---

## Key Services Summary

### Core GenAI Services
| Service | Purpose |
|---------|---------|
| Amazon Bedrock | FM access and management |
| Bedrock Knowledge Bases | Managed RAG |
| Bedrock Agents | Agentic workflows |
| Bedrock Guardrails | Safety controls |

### Supporting Services
| Service | Purpose |
|---------|---------|
| OpenSearch Serverless | Vector search |
| Aurora PostgreSQL (pgvector) | Vector database |
| SageMaker | Custom model deployment |
| Lambda | Serverless inference |
| API Gateway | API management |
| CloudWatch | Monitoring |

---

## Key Documentation Links

### Amazon Bedrock
- [Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/)
- [Knowledge Bases](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-bases.html)
- [Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
- [Model Parameters](https://docs.aws.amazon.com/bedrock/latest/userguide/model-parameters.html)
- [Prompt Management](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-management.html)

### Vector Search
- [OpenSearch Serverless Vector Search](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/serverless-vector-search.html)
- [Aurora PostgreSQL pgvector](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraPostgreSQL.VectorDB.html)

### Security
- [Bedrock IAM](https://docs.aws.amazon.com/bedrock/latest/userguide/security_iam_service-with-iam.html)
- [Model Invocation Logging](https://docs.aws.amazon.com/bedrock/latest/userguide/model-invocation-logging.html)

### Deployment
- [SageMaker Deployment Options](https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model-options.html)
- [Cross-Region Inference](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html)

---

## Study Tips

1. **Hands-on with Bedrock** - Use the console and APIs
2. **Build a RAG application** - End-to-end with Knowledge Bases
3. **Implement Guardrails** - Understand safety controls
4. **Practice prompt engineering** - Different techniques and chains
5. **Understand pricing** - Token-based vs provisioned throughput
