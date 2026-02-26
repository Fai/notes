---
tags: [aws, ecs, fargate, containers, troubleshooting]
created: 2025-12-18
updated: 2025-12-18
---

# ECS Troubleshooting Guide

Common issues and solutions for Amazon ECS deployments.

## Health Check Failures

### Symptoms
- Tasks failing health checks
- Service degraded (fewer healthy tasks)
- Exit code 0 (graceful shutdown by ECS)
- Deployment circuit breaker triggered

### Common Causes

#### 1. Health Check Too Aggressive
```json
// Too aggressive (default)
{
  "timeout": 5,
  "unhealthyThreshold": 2
}

// More forgiving
{
  "timeout": 30,
  "interval": 60,
  "unhealthyThreshold": 3,
  "healthyThreshold": 2
}
```

#### 2. Application Overload
- Message queue flood (MQTT, SQS, Kafka)
- Database connection exhaustion
- Thread pool saturation

**Impact Chain:**
```
Message flood → DB queries → Thread saturation → 
Health check timeout → Task killed → More load per task → Failure
```

#### 3. Insufficient Cluster Capacity
```bash
# Check cluster capacity
aws ecs describe-clusters --clusters <cluster> \
  --include STATISTICS

# Calculate max tasks
# Total CPU / Task CPU = Max tasks
# Example: 16384 / 2560 = 6 tasks max
```

### Diagnostic Commands
```bash
# Check service events
aws ecs describe-services --cluster <cluster> --services <service> \
  --query 'services[].events[:10]'

# Check stopped task reasons
aws ecs describe-tasks --cluster <cluster> --tasks <task-arn> \
  --query 'tasks[].stoppedReason'

# Check target health
aws elbv2 describe-target-health --target-group-arn <tg-arn>
```

## Deployment Circuit Breaker

### When It Triggers
- Multiple consecutive task failures
- Tasks can't start due to resource constraints
- Health checks failing repeatedly

### Resolution
```bash
# Check deployment status
aws ecs describe-services --cluster <cluster> --services <service> \
  --query 'services[].deployments'

# Force new deployment
aws ecs update-service --cluster <cluster> --service <service> \
  --force-new-deployment

# Rollback if needed
aws ecs update-service --cluster <cluster> --service <service> \
  --task-definition <previous-task-def>
```

## Resource Constraints

### CPU/Memory Limits
```bash
# Check task resource usage
aws ecs describe-tasks --cluster <cluster> --tasks <task-arn> \
  --query 'tasks[].containers[].{name:name,cpu:cpu,memory:memory}'

# Check container insights (if enabled)
aws logs filter-log-events --log-group-name /aws/ecs/containerinsights/<cluster>/performance
```

### Scaling Recommendations
- Don't scale beyond cluster capacity
- Use Auto Scaling with proper min/max
- Consider Fargate for elastic capacity

## Networking Issues

### ENI Provisioning (Fargate)
- awsvpc mode adds ENI provisioning latency
- Can take several seconds per task
- Limit: ~250 ENIs per subnet

### DNS Resolution Limits
- Fargate: 1024 packets/sec DNS limit
- Can bottleneck during high connection churn
- Solution: Use connection pooling

## Related
- [RDS Proxy Connections](./rds-proxy-troubleshooting.md)
- [Database Connections](../troubleshooting/database-connections.md)
