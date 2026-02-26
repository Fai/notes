# Knowledge Base

Reusable technical knowledge extracted from project work.

## AWS

### Databases
- [RDS Connectivity Troubleshooting](aws/rds-connectivity-troubleshooting.md) - Decision tree, SSL, security groups
- [RDS Proxy Troubleshooting](aws/rds-proxy-troubleshooting.md) - TLS certificates, connection limits, pinning
- [Redis & ElastiCache in Kubernetes](aws/redis-elasticache-kubernetes.md) - Lettuce IP caching, cluster setup

### Compute & Containers
- [ECS Troubleshooting](aws/ecs-troubleshooting.md) - Health checks, circuit breaker, scaling
- [EBS GP3 Migration](aws/ebs-gp3-migration.md) - StorageClass, migration steps, cost savings

### Messaging
- [Kafka/MSK Troubleshooting](aws/kafka-msk-troubleshooting.md) - "Brokers down" warnings, consumer lag

### AI/ML
- [Bedrock Knowledge Bases](aws/bedrock-knowledge-bases.md) - KB types, costs, implementation patterns
- [Bedrock Cost Control](aws/bedrock-cost-control.md) - Per-user token limits, budget alerts, throttling
- [AI/ML Governance](aws/ai-ml-governance.md) - SageMaker Catalog, data lineage, access control

### Account & Administration
- [Account Closure Automation](aws/account-closure-automation.md) - Batch closure, quotas, cleanup tools
- [QuickSight Administration](aws/quicksight-administration.md) - Batch unsubscribe, cross-account operations

### Security
- [Security Incident Investigation](aws/security-incident-investigation.md) - CloudTrail queries, attack patterns

## Kubernetes
- [EKS Node Management](kubernetes/eks-node-management.md) - Karpenter, Auto Mode, resource requests

## Troubleshooting
- [Database Connections](troubleshooting/database-connections.md) - Diagnostic flow, connection pools

---

## Contributing

When you solve a problem that could apply to other projects:
1. Extract the generic solution (remove customer-specific details)
2. Add to appropriate category
3. Link from customer docs using relative paths

Example link in customer docs:
```markdown
## Related
- See [RDS Troubleshooting](/knowledge-base/aws/rds-connectivity-troubleshooting.md)
```
