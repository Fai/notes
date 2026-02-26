---
title: "EKS Node Management Guide"
title: "EKS Node Management"
tags: [aws, eks, kubernetes, karpenter, autoscaling]
created: 2025-12-18
updated: 2025-12-18
---
title: "EKS Node Management"

# EKS Node Management Guide

Options for managing EKS compute resources: Managed Node Groups, Karpenter, and EKS Auto Mode.

## Comparison

| Feature | Managed Node Groups | Karpenter | EKS Auto Mode |
|---------|---------------------|-----------|---------------|
| Setup Complexity | Low | Medium | Low |
| Instance Selection | Manual | Automatic | Automatic |
| Scaling Speed | Minutes | Seconds | Seconds |
| Cost Optimization | Manual | High | High |
| Spot Support | Yes | Yes | Yes |
| Maintenance | Manual AMI updates | Self-managed | AWS-managed |

## Resource Requests/Limits (Critical)

All autoscaling solutions need accurate resource requests:

```yaml
# Standard microservice
resources:
  requests:
    cpu: "200m"
    memory: "512Mi"
  limits:
    cpu: "500m"
    memory: "1Gi"

# Memory-heavy services
resources:
  requests:
    cpu: "500m"
    memory: "4Gi"
  limits:
    cpu: "1000m"
    memory: "6Gi"
```

## Karpenter Setup

### Prerequisites
- EKS cluster 1.21+
- Node IAM role with EC2 permissions
- Karpenter controller IAM role

### NodePool Example
```yaml
apiVersion: karpenter.sh/v1beta1
kind: NodePool
metadata:
  name: default
spec:
  template:
    spec:
      requirements:
        - key: kubernetes.io/arch
          operator: In
          values: ["amd64"]
        - key: karpenter.sh/capacity-type
          operator: In
          values: ["on-demand", "spot"]
        - key: node.kubernetes.io/instance-type
          operator: In
          values: ["m5.large", "m5.xlarge", "m5.2xlarge"]
      nodeClassRef:
        name: default
  limits:
    cpu: 100
    memory: 200Gi
  disruption:
    consolidationPolicy: WhenUnderutilized
```

## EKS Auto Mode

Simplest option - AWS manages everything:
1. Enable Auto Mode in cluster settings
2. Add resource requests to all pods
3. Delete managed node groups
4. Auto Mode provisions nodes automatically

## Migration Checklist

- [ ] Audit all pods for resource requests/limits
- [ ] Backup current node group configuration
- [ ] Test in non-production first
- [ ] Plan maintenance window
- [ ] Monitor after migration

## Useful Commands

```bash
# Check pod resource usage
kubectl top pods -A

# Review pod resource requests
kubectl get pods -A -o json | jq '.items[] | {name: .metadata.name, resources: .spec.containers[].resources}'

# Check node capacity
kubectl describe nodes | grep -A 5 "Allocated resources"

# Monitor Karpenter logs
kubectl logs -n karpenter -l app.kubernetes.io/name=karpenter -f
```

## Related
- Karpenter vs EKS Auto Mode Comparison
- Node Group AMI Updates
