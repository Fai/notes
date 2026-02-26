---
title: "Kafka MSK Troubleshooting"
title: "Kafka MSK Troubleshooting"
tags: [aws, kafka, msk, messaging, troubleshooting]
created: 2025-12-18
updated: 2025-12-18
---
title: "Kafka MSK Troubleshooting"

# Kafka / MSK Troubleshooting

Common issues and solutions for Apache Kafka and Amazon MSK.

## "Brokers Down" Warnings (False Positives)

### Symptoms
```
ERROR: 2/2 brokers are down
```
- Occurs at exact intervals (e.g., every 10 minutes)
- No actual message processing failures
- Pattern is predictable, not random

### Root Cause
rdkafka library metadata refresh timeout, NOT actual broker failures.

**Default Configuration:**
```
metadata.max.age.ms = 600000  (10 minutes)
```

### Why It Happens
1. Client initiates metadata refresh every 10 minutes
2. Network latency spike or broker busy
3. Request times out before response
4. Warning logged, retry succeeds

### Solutions

#### Option 1: Increase Timeout (Recommended)
```yaml
# Kafka client config
socket.timeout.ms: 60000
metadata.request.timeout.ms: 30000
```

#### Option 2: Reduce Refresh Frequency
```yaml
metadata.max.age.ms: 900000  # 15 minutes
```

#### Option 3: Accept as Normal
If messages are processing correctly, these warnings can be ignored.

## Consumer Group Issues

### Consumer Lag Building Up
```bash
# Check consumer lag
kafka-consumer-groups.sh --bootstrap-server <broker> \
  --describe --group <group-id>
```

**Common Causes:**
- Slow message processing
- Too few consumers for partition count
- Consumer rebalancing frequently

**Solutions:**
- Increase consumer instances
- Optimize message processing
- Tune `max.poll.records` and `max.poll.interval.ms`

### Consumer Rebalancing Frequently
```yaml
# Increase session timeout
session.timeout.ms: 45000
heartbeat.interval.ms: 15000
max.poll.interval.ms: 300000
```

## MSK-Specific Issues

### Connection from EKS
```yaml
# Security group requirements
- Inbound: Port 9092 (plaintext) or 9094 (TLS)
- Source: EKS node security group
```

### MSK Serverless vs Provisioned
| Aspect | Serverless | Provisioned |
|--------|------------|-------------|
| Scaling | Automatic | Manual |
| Cost | Pay per use | Fixed capacity |
| Max partitions | 120 | Unlimited |
| Best for | Variable load | Predictable load |

## Monitoring

Key metrics to watch:
- `BytesInPerSec` / `BytesOutPerSec`
- `UnderReplicatedPartitions` (should be 0)
- `OfflinePartitionsCount` (should be 0)
- Consumer lag per group

```bash
# Quick health check
kafka-metadata.sh --snapshot /var/kafka-logs/__cluster_metadata-0/00000000000000000000.log --command "describe"
```

## Related
- [EKS Node Management](../kubernetes/eks-node-management.md)
- [Database Connections Troubleshooting](../troubleshooting/database-connections.md)
