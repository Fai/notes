---
title: "Redis & ElastiCache in Kubernetes"
title: "Redis ElastiCache on Kubernetes"
tags: [aws, redis, elasticache, kubernetes, caching]
created: 2025-12-18
updated: 2025-12-18
---
title: "Redis ElastiCache on Kubernetes"

# Redis & ElastiCache in Kubernetes

Common issues and solutions for Redis deployments in Kubernetes/EKS.

## Lettuce Client IP Caching Issue

### Problem
Lettuce Redis client caches cluster topology and node IPs. When Redis pods restart with new IPs, Lettuce continues connecting to stale IPs.

### Symptoms
- `RedisConnectionException: Unable to connect to <old-ip>:6379`
- Connection errors after Redis pod restarts
- Works after application restart

### Solutions

#### 1. Enable Topology Refresh (Recommended)
```java
ClusterClientOptions options = ClusterClientOptions.builder()
    .topologyRefreshOptions(ClusterTopologyRefreshOptions.builder()
        .enablePeriodicRefresh(Duration.ofMinutes(1))
        .enableAllAdaptiveRefreshTriggers()
        .build())
    .build();
```

#### 2. Use Kubernetes Service DNS
```yaml
env:
- name: REDIS_HOST
  value: redis-cluster.namespace.svc.cluster.local
```

#### 3. Configure DNS TTL
```yaml
# In application config
spring.redis.lettuce.cluster.refresh.adaptive: true
spring.redis.lettuce.cluster.refresh.period: 60s
```

## Redis Cluster on Kubernetes

### Headless Service Pattern
```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-cluster
spec:
  clusterIP: None  # Headless
  ports:
  - port: 6379
    name: client
  - port: 16379
    name: gossip
  selector:
    app: redis-cluster
```

### StatefulSet with PVC
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: redis-cluster
spec:
  serviceName: redis-cluster
  replicas: 6
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      storageClassName: gp3
      resources:
        requests:
          storage: 10Gi
```

## ElastiCache vs Self-Managed

| Aspect | ElastiCache | Self-Managed (K8s) |
|--------|-------------|-------------------|
| Management | AWS-managed | You manage |
| Cost | Higher | Lower |
| HA | Built-in | Configure yourself |
| Scaling | Easy | Manual |
| Network | VPC only | Pod network |

## Common Issues

### 1. OOM (Out of Memory)
```bash
# Check memory usage
redis-cli INFO memory

# Set maxmemory policy
CONFIG SET maxmemory-policy allkeys-lru
```

### 2. Connection Limits
```bash
# Check connections
redis-cli CLIENT LIST | wc -l

# Set max clients
CONFIG SET maxclients 10000
```

### 3. Slow Commands
```bash
# Enable slow log
CONFIG SET slowlog-log-slower-than 10000
SLOWLOG GET 10
```

## Monitoring

Key metrics to watch:
- `used_memory` / `maxmemory`
- `connected_clients`
- `blocked_clients`
- `keyspace_hits` / `keyspace_misses`
- `evicted_keys`

## Related
- Redis PVC Migration Guide
- Kubernetes StatefulSet Best Practices
