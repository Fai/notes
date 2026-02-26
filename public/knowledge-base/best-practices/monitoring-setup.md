---
title: "Monitoring Setup Guide"
title: "Monitoring Setup"
tags: [best-practices, monitoring, aws, observability]
created: 2025-12-18
updated: 2025-12-18
---
title: "Monitoring Setup"

# Monitoring Setup Guide

Essential monitoring configuration for AWS workloads.

## Core Metrics to Monitor

### Application Layer
| Metric | Warning | Critical | Action |
|--------|---------|----------|--------|
| Error rate (5xx) | >1% | >5% | Check logs, scale |
| Response time p99 | >2s | >5s | Optimize, scale |
| Request rate | Baseline +50% | Baseline +100% | Scale out |
| Active connections | >80% max | >95% max | Scale, optimize |

### Compute (ECS/EC2)
| Metric | Warning | Critical | Action |
|--------|---------|----------|--------|
| CPU utilization | >70% | >85% | Scale out |
| Memory utilization | >75% | >90% | Scale out/up |
| Task count | <desired | 0 | Investigate |

### Database (RDS)
| Metric | Warning | Critical | Action |
|--------|---------|----------|--------|
| CPU utilization | >70% | >85% | Scale up, optimize queries |
| Connections | >80% max | >95% max | Connection pooling |
| Free storage | <20% | <10% | Increase storage |
| Read/Write latency | >20ms | >100ms | Optimize, scale |

### Cache (ElastiCache)
| Metric | Warning | Critical | Action |
|--------|---------|----------|--------|
| CPU utilization | >65% | >80% | Scale out |
| Memory usage | >75% | >90% | Scale, eviction policy |
| Cache hit ratio | <80% | <60% | Review caching strategy |
| Evictions | >0 sustained | High rate | Increase memory |

## CloudWatch Alarms Setup

### ECS Service Alarms
```bash
# High CPU alarm
aws cloudwatch put-metric-alarm \
  --alarm-name "${ENV}-ecs-high-cpu" \
  --alarm-description "ECS CPU > 80%" \
  --metric-name CPUUtilization \
  --namespace AWS/ECS \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --dimensions Name=ServiceName,Value=${SERVICE_NAME} Name=ClusterName,Value=${CLUSTER_NAME} \
  --alarm-actions ${SNS_TOPIC_ARN}

# Unhealthy task count
aws cloudwatch put-metric-alarm \
  --alarm-name "${ENV}-ecs-unhealthy-tasks" \
  --alarm-description "ECS unhealthy tasks" \
  --metric-name UnHealthyHostCount \
  --namespace AWS/ApplicationELB \
  --statistic Sum \
  --period 60 \
  --threshold 1 \
  --comparison-operator GreaterThanOrEqualToThreshold \
  --evaluation-periods 2 \
  --dimensions Name=TargetGroup,Value=${TARGET_GROUP_ARN} \
  --alarm-actions ${SNS_TOPIC_ARN}
```

### RDS Alarms
```bash
# High connections
aws cloudwatch put-metric-alarm \
  --alarm-name "${ENV}-rds-high-connections" \
  --alarm-description "RDS connections > 80%" \
  --metric-name DatabaseConnections \
  --namespace AWS/RDS \
  --statistic Average \
  --period 300 \
  --threshold ${MAX_CONNECTIONS_80_PERCENT} \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --dimensions Name=DBInstanceIdentifier,Value=${DB_INSTANCE} \
  --alarm-actions ${SNS_TOPIC_ARN}

# Low storage
aws cloudwatch put-metric-alarm \
  --alarm-name "${ENV}-rds-low-storage" \
  --alarm-description "RDS storage < 10GB" \
  --metric-name FreeStorageSpace \
  --namespace AWS/RDS \
  --statistic Average \
  --period 300 \
  --threshold 10737418240 \
  --comparison-operator LessThanThreshold \
  --evaluation-periods 1 \
  --dimensions Name=DBInstanceIdentifier,Value=${DB_INSTANCE} \
  --alarm-actions ${SNS_TOPIC_ARN}
```

## CloudWatch Dashboard

```json
{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "title": "ECS CPU/Memory",
        "metrics": [
          ["AWS/ECS", "CPUUtilization", "ServiceName", "${SERVICE}", "ClusterName", "${CLUSTER}"],
          [".", "MemoryUtilization", ".", ".", ".", "."]
        ],
        "period": 300,
        "stat": "Average"
      }
    },
    {
      "type": "metric",
      "properties": {
        "title": "ALB Request Count & Latency",
        "metrics": [
          ["AWS/ApplicationELB", "RequestCount", "LoadBalancer", "${ALB}"],
          [".", "TargetResponseTime", ".", ".", {"stat": "p99"}]
        ]
      }
    },
    {
      "type": "metric",
      "properties": {
        "title": "RDS Performance",
        "metrics": [
          ["AWS/RDS", "CPUUtilization", "DBInstanceIdentifier", "${DB}"],
          [".", "DatabaseConnections", ".", "."],
          [".", "ReadLatency", ".", "."],
          [".", "WriteLatency", ".", "."]
        ]
      }
    }
  ]
}
```

## Log Aggregation

### CloudWatch Logs Insights Queries

```sql
-- Error rate by service
fields @timestamp, @message
| filter @message like /ERROR/
| stats count() as errors by bin(5m)

-- Slow requests
fields @timestamp, @message, duration
| filter duration > 1000
| sort duration desc
| limit 20

-- Top error messages
fields @message
| filter @message like /ERROR/
| stats count() by @message
| sort count desc
| limit 10
```

### Log Retention Policy
| Log Type | Retention | Reason |
|----------|-----------|--------|
| Application logs | 30 days | Troubleshooting |
| Access logs | 90 days | Security audit |
| CloudTrail | 1 year | Compliance |
| VPC Flow Logs | 14 days | Network debug |

## Alerting Strategy

### Notification Channels
1. **P1 (Critical)**: PagerDuty/OpsGenie → Phone call
2. **P2 (Warning)**: Slack → #alerts channel
3. **P3 (Info)**: Email → ops team

### Alert Fatigue Prevention
- Use composite alarms
- Set appropriate thresholds (not too sensitive)
- Implement alert suppression during maintenance
- Review and tune alerts monthly

## Health Check Endpoints

```json
// /health response
{
  "status": "healthy",
  "version": "1.0.0",
  "timestamp": "2025-12-18T10:00:00Z",
  "dependencies": {
    "database": "healthy",
    "cache": "healthy",
    "external_api": "healthy"
  }
}
```

## Related

- [Incident Response](/best-practices/incident-response.md)
- [ECS Troubleshooting](/knowledge-base/aws/ecs-troubleshooting.md)
- [RDS Connectivity](/knowledge-base/aws/rds-connectivity-troubleshooting.md)
