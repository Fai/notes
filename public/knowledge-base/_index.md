# Knowledge Base

Reusable technical knowledge extracted from project work.

## AWS

### AI/ML (Bedrock & LLM)
- [Self-Hosted LLM Best Practices](aws/self-hosted-llm.md) — GPU sizing, vLLM, KV cache, hybrid architecture
- [Bedrock Knowledge Bases](aws/bedrock-knowledge-bases.md) — KB types, costs, implementation patterns
- [Bedrock Cost Control](aws/bedrock-cost-control.md) — Per-user token limits, budget alerts, throttling
- [AI/ML Governance](aws/ai-ml-governance.md) — SageMaker Catalog, data lineage, access control

### Databases
- [RDS Connectivity Troubleshooting](aws/rds-connectivity-troubleshooting.md) — Decision tree, SSL, security groups
- [RDS Proxy Troubleshooting](aws/rds-proxy-troubleshooting.md) — TLS certificates, connection limits, pinning
- [Redis & ElastiCache in Kubernetes](aws/redis-elasticache-kubernetes.md) — Lettuce IP caching, cluster setup

### Compute & Containers
- [ECS Troubleshooting](aws/ecs-troubleshooting.md) — Health checks, circuit breaker, scaling
- [EBS GP3 Migration](aws/ebs-gp3-migration.md) — StorageClass, migration steps, cost savings

### Messaging
- [Kafka/MSK Troubleshooting](aws/kafka-msk-troubleshooting.md) — "Brokers down" warnings, consumer lag

### Account & Administration
- [Account Closure Automation](aws/account-closure-automation.md) — Batch closure, quotas, cleanup tools
- [QuickSight Administration](aws/quicksight-administration.md) — Batch unsubscribe, cross-account operations

### Security
- [Security Incident Investigation](aws/security-incident-investigation.md) — CloudTrail queries, attack patterns

## Kubernetes

- [EKS Node Management](kubernetes/eks-node-management.md) — Karpenter, Auto Mode, resource requests

## Troubleshooting

- [Database Connections](troubleshooting/database-connections.md) — Diagnostic flow, connection pools

## Best Practices

Checklists and frameworks for recurring operational tasks.

- [Azure to AWS Migration](best-practices/azure-to-aws-migration.md)
- [Migration Checklist](best-practices/migration-checklist.md)
- [Security Checklist](best-practices/security-checklist.md)
- [Monitoring Setup](best-practices/monitoring-setup.md)
- [Incident Response](best-practices/incident-response.md)
- [Knowledge Extraction Checklist](best-practices/knowledge-extraction-checklist.md)
- [Quarterly Review Checklist](best-practices/quarterly-review-checklist.md)
- [Cost Optimization](best-practices/cost-optimization.md)
