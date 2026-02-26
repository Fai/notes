---
tags: [aws, ebs, storage, migration, kubernetes]
created: 2025-12-18
updated: 2025-12-18
---

# EBS GP3 Migration Guide

Migrating from gp2 to gp3 for better performance and cost savings.

## GP2 vs GP3 Comparison

| Feature | gp2 | gp3 |
|---------|-----|-----|
| Baseline IOPS | 3 IOPS/GB | 3,000 IOPS (fixed) |
| Max IOPS | 16,000 | 16,000 |
| Baseline Throughput | 128-250 MB/s | 125 MB/s |
| Max Throughput | 250 MB/s | 1,000 MB/s |
| Cost | $0.10/GB | $0.08/GB |
| IOPS/Throughput | Tied to size | Independent |

**Cost Savings:** ~20% for most workloads

## Kubernetes StorageClass

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: gp3
provisioner: ebs.csi.aws.com
parameters:
  type: gp3
  fsType: ext4
  iops: "3000"
  throughput: "125"
volumeBindingMode: WaitForFirstConsumer
allowVolumeExpansion: true
reclaimPolicy: Retain
```

## Prerequisites

### EBS CSI Driver
```bash
# Check if installed
kubectl get pods -n kube-system | grep ebs-csi

# Install if missing
helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
helm upgrade --install aws-ebs-csi-driver \
  --namespace kube-system \
  aws-ebs-csi-driver/aws-ebs-csi-driver
```

## Migration Approaches

### Option 1: New PVC (Zero Downtime)
For StatefulSets with data that can be replicated:

1. Create gp3 StorageClass
2. Update StatefulSet volumeClaimTemplates
3. Rolling restart pods
4. Data replicates to new volumes

### Option 2: Volume Modification (In-Place)
For existing volumes:

```bash
# Modify volume type
aws ec2 modify-volume --volume-id vol-xxx --volume-type gp3

# Check progress
aws ec2 describe-volumes-modifications --volume-id vol-xxx
```

**Note:** Volume modification can take hours for large volumes.

### Option 3: Snapshot and Restore
For critical data:

```bash
# Create snapshot
aws ec2 create-snapshot --volume-id vol-xxx --description "Pre-migration backup"

# Create new gp3 volume from snapshot
aws ec2 create-volume --snapshot-id snap-xxx --volume-type gp3 --availability-zone us-east-1a
```

## Redis Cluster Migration Example

```bash
# 1. Backup
kubectl exec redis-cluster-0 -- redis-cli BGSAVE
kubectl cp redis-cluster-0:/data/dump.rdb ./backup-redis-0.rdb

# 2. Apply StorageClass
kubectl apply -f storageclass-gp3.yaml

# 3. Update StatefulSet
kubectl apply -f redis-statefulset-gp3.yaml

# 4. Rolling restart (one at a time)
for i in 2 1 0; do
  kubectl delete pod redis-cluster-$i
  kubectl wait --for=condition=ready pod/redis-cluster-$i --timeout=300s
done

# 5. Verify
kubectl exec redis-cluster-0 -- redis-cli cluster info
```

## Verification

```bash
# Check PVC storage class
kubectl get pvc -o custom-columns=NAME:.metadata.name,STORAGECLASS:.spec.storageClassName

# Check actual volume type
aws ec2 describe-volumes --filters "Name=tag:kubernetes.io/created-for/pvc/name,Values=*" \
  --query 'Volumes[].{ID:VolumeId,Type:VolumeType,Size:Size}'

# Check IOPS/Throughput
aws ec2 describe-volumes --volume-ids vol-xxx \
  --query 'Volumes[].{IOPS:Iops,Throughput:Throughput}'
```

## Rollback

If issues occur:
1. Keep old PVCs until validation complete
2. Scale down, switch back to old PVC
3. Scale up

## Related
- [EKS Node Management](../kubernetes/eks-node-management.md)
- [Migration Checklist](/best-practices/migration-checklist.md)
