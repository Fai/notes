---
title: "RDS Proxy Troubleshooting Guide"
title: "RDS Proxy Troubleshooting"
tags: [aws, rds, rds-proxy, database, troubleshooting]
created: 2025-12-18
updated: 2025-12-18
---
title: "RDS Proxy Troubleshooting"

# RDS Proxy Troubleshooting Guide

Common issues and solutions for Amazon RDS Proxy.

## Connection Limit Issues

### Symptoms
- Connections capping at ~500 despite higher limits
- HTTP 500 errors during peak traffic
- Works in load test, fails in production

### Root Causes

#### 1. TLS/SSL Certificate Mismatch
RDS Proxy uses **different certificates** than direct RDS connections.

**Direct RDS:** Amazon RDS CA (rds-ca-rsa2048-g1)
**RDS Proxy:** Starfield Services Root CA (via ACM)

**Solution:**
```bash
# Download global bundle (includes Starfield)
curl -o /app/certs/global-bundle.pem \
    https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem
```

**Connection String (.NET):**
```csharp
var builder = new NpgsqlConnectionStringBuilder
{
    Host = "proxy.proxy-xxxxx.region.rds.amazonaws.com",
    SslMode = SslMode.VerifyFull,
    RootCertificate = "/app/certs/global-bundle.pem"
};
```

#### 2. Connection Leaks
Connections opened but not properly disposed.

**Detection:**
```sql
-- PostgreSQL: Check connection age
SELECT pid, usename, application_name, 
       now() - backend_start as connection_age
FROM pg_stat_activity 
ORDER BY connection_age DESC;
```

**Solution:** Ensure proper connection disposal:
```csharp
// C# - Use 'using' statement
using (var conn = new NpgsqlConnection(connectionString))
{
    await conn.OpenAsync();
    // ... use connection
} // Automatically disposed
```

#### 3. MaxConnectionsPercent Misconfiguration
```bash
# Check current setting
aws rds describe-db-proxies --db-proxy-name <proxy> \
  --query 'DBProxies[].ConnectionPoolConfig'

# Recommended: Start with 50-70%, not 100%
aws rds modify-db-proxy --db-proxy-name <proxy> \
  --connection-pool-config MaxConnectionsPercent=70
```

## TLS Handshake Bottleneck

### Symptoms
- Connection failures during scaling events
- Slow connection establishment
- Works with few connections, fails at scale

### Solutions

#### 1. Connection Pooling
```yaml
# Application-side pool settings
MinPoolSize: 5
MaxPoolSize: 20
ConnectionIdleLifetime: 300
ConnectionPruningInterval: 10
```

#### 2. Pre-warm Connections
```csharp
// Pre-warm pool on startup
for (int i = 0; i < minPoolSize; i++)
{
    using var conn = new NpgsqlConnection(connectionString);
    await conn.OpenAsync();
}
```

#### 3. Reduce TLS Overhead
```csharp
// If in VPC with private endpoint, consider:
SslMode = SslMode.Prefer  // Falls back to non-SSL in VPC
```

## Monitoring

### Key Metrics
- `DatabaseConnectionsCurrentlySessionPinned`
- `DatabaseConnections`
- `ClientConnections`
- `QueryRequests`
- `QueryDatabaseResponseLatency`

### CloudWatch Query
```sql
SELECT AVG(DatabaseConnections), MAX(ClientConnections)
FROM SCHEMA("AWS/RDS", DBProxyName)
WHERE DBProxyName = 'your-proxy'
GROUP BY BIN(5m)
```

## Session Pinning

### What Causes Pinning
- Prepared statements
- Session variables
- Temporary tables
- Advisory locks

### Detection
```bash
# Check pinned connections
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name DatabaseConnectionsCurrentlySessionPinned \
  --dimensions Name=DBProxyName,Value=<proxy> \
  --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --period 300 \
  --statistics Average
```

### Mitigation
- Avoid session-level features when possible
- Use connection-level prepared statements
- Close connections after session-specific operations

## Related
- [RDS Connectivity Troubleshooting](./rds-connectivity-troubleshooting.md)
- [ECS Troubleshooting](./ecs-troubleshooting.md)
- [Database Connections](../troubleshooting/database-connections.md)
