---
tags: [best-practices, cost, aws, optimization]
created: 2025-12-18
updated: 2025-12-18
---

# AWS Cost Optimization Guide

Strategies for reducing AWS costs without sacrificing performance.

## Quick Wins (Immediate Savings)

### 1. Right-Size Instances
```bash
# Check underutilized EC2 instances
aws ce get-rightsizing-recommendation \
  --service EC2 \
  --configuration RecommendationTarget=SAME_INSTANCE_FAMILY
```

### 2. Delete Unused Resources
- [ ] Unattached EBS volumes
- [ ] Unused Elastic IPs
- [ ] Old snapshots (>30 days)
- [ ] Unused load balancers
- [ ] Stopped EC2 instances

```bash
# Find unattached EBS volumes
aws ec2 describe-volumes --filters Name=status,Values=available \
  --query 'Volumes[].{ID:VolumeId,Size:Size,Created:CreateTime}'

# Find unused Elastic IPs
aws ec2 describe-addresses --query 'Addresses[?AssociationId==null]'
```

### 3. Use Spot Instances
- Dev/test environments: 60-90% savings
- Batch processing: Use Spot Fleet
- EKS: Use Karpenter with Spot

## Compute Optimization

### EC2
| Strategy | Savings | Use Case |
|----------|---------|----------|
| Reserved Instances (1yr) | 30-40% | Steady-state workloads |
| Reserved Instances (3yr) | 50-60% | Long-term commitments |
| Savings Plans | 30-40% | Flexible compute |
| Spot Instances | 60-90% | Fault-tolerant workloads |

### ECS/Fargate
- Use Fargate Spot for non-critical tasks
- Right-size task CPU/memory
- Use ARM64 (Graviton) for 20% savings

### Lambda
- Right-size memory allocation
- Use ARM64 architecture
- Optimize cold starts
- Use Provisioned Concurrency sparingly

## Storage Optimization

### S3
| Storage Class | Use Case | Cost |
|---------------|----------|------|
| Standard | Frequent access | $0.023/GB |
| Intelligent-Tiering | Unknown patterns | $0.023/GB + monitoring |
| Standard-IA | Infrequent access | $0.0125/GB |
| Glacier Instant | Archive, instant access | $0.004/GB |
| Glacier Deep Archive | Long-term archive | $0.00099/GB |

```bash
# Set up lifecycle policy
aws s3api put-bucket-lifecycle-configuration \
  --bucket my-bucket \
  --lifecycle-configuration '{
    "Rules": [{
      "ID": "Move to IA after 30 days",
      "Status": "Enabled",
      "Filter": {"Prefix": ""},
      "Transitions": [
        {"Days": 30, "StorageClass": "STANDARD_IA"},
        {"Days": 90, "StorageClass": "GLACIER"}
      ]
    }]
  }'
```

### EBS
- Use gp3 instead of gp2 (20% cheaper)
- Delete old snapshots
- Use smaller volumes with higher IOPS

## Database Optimization

### RDS
- Use Reserved Instances for production
- Use Aurora Serverless v2 for variable workloads
- Enable storage autoscaling
- Use read replicas instead of scaling up

### ElastiCache
- Use Reserved Nodes
- Right-size node types
- Use Serverless for variable workloads

## Network Optimization

- Use VPC endpoints (avoid NAT Gateway costs)
- Use CloudFront for static content
- Minimize cross-AZ traffic
- Use S3 Transfer Acceleration only when needed

## Monitoring & Alerts

### Set Up Budget Alerts
```bash
aws budgets create-budget \
  --account-id $(aws sts get-caller-identity --query Account --output text) \
  --budget '{
    "BudgetName": "Monthly-Budget",
    "BudgetLimit": {"Amount": "1000", "Unit": "USD"},
    "TimeUnit": "MONTHLY",
    "BudgetType": "COST"
  }' \
  --notifications-with-subscribers '[{
    "Notification": {
      "NotificationType": "ACTUAL",
      "ComparisonOperator": "GREATER_THAN",
      "Threshold": 80
    },
    "Subscribers": [{
      "SubscriptionType": "EMAIL",
      "Address": "alerts@example.com"
    }]
  }]'
```

### Cost Explorer Queries
```bash
# Get cost by service
aws ce get-cost-and-usage \
  --time-period Start=$(date -d '30 days ago' +%Y-%m-%d),End=$(date +%Y-%m-%d) \
  --granularity MONTHLY \
  --metrics BlendedCost \
  --group-by Type=DIMENSION,Key=SERVICE
```

## Monthly Review Checklist

- [ ] Review Cost Explorer for anomalies
- [ ] Check for unused resources
- [ ] Review Reserved Instance utilization
- [ ] Check Savings Plans coverage
- [ ] Review data transfer costs
- [ ] Optimize top 5 cost drivers

## Related

- [EBS GP3 Migration](/knowledge-base/aws/ebs-gp3-migration.md)
- [Bedrock Knowledge Bases](/knowledge-base/aws/bedrock-knowledge-bases.md) (cost comparison)
