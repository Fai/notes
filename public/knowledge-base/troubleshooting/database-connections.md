---
tags: [database, troubleshooting, connectivity, connection-pool]
created: 2025-12-18
updated: 2025-12-18
---

# Database Connection Troubleshooting

Systematic approach to diagnosing database connectivity issues.

## Diagnostic Flow

```
Connection Issue
│
├─ 1. Network Layer
│   ├─ Security Groups
│   ├─ NACLs
│   └─ Route Tables
│
├─ 2. DNS Resolution
│   ├─ VPC DNS settings
│   └─ Endpoint correctness
│
├─ 3. Authentication
│   ├─ Credentials
│   ├─ SSL/TLS config
│   └─ User permissions
│
└─ 4. Application Layer
    ├─ Connection pool
    ├─ Connection leaks
    └─ Timeout settings
```

## Quick Diagnostics

### 1. Network Connectivity
```bash
# Test TCP connection
nc -zv <db-endpoint> <port>

# Test from container
kubectl exec -it <pod> -- nc -zv <db-endpoint> <port>

# Check DNS
nslookup <db-endpoint>
dig <db-endpoint>
```

### 2. Security Group Check
```bash
# List security group rules
aws ec2 describe-security-groups --group-ids <sg-id> \
  --query 'SecurityGroups[].IpPermissions'
```

### 3. Connection Pool Analysis
```sql
-- PostgreSQL: Check connections
SELECT count(*) FROM pg_stat_activity;
SELECT * FROM pg_stat_activity WHERE state = 'active';

-- MySQL: Check connections
SHOW PROCESSLIST;
SHOW STATUS LIKE 'Threads_connected';
```

## Common Patterns

### Pattern 1: Works Locally, Fails in Container
**Cause:** Security group doesn't allow container network
**Fix:** Add inbound rule for ECS/EKS security group

### Pattern 2: Intermittent Timeouts
**Cause:** Connection pool exhaustion or leaks
**Fix:** 
- Add connection pool monitoring
- Set proper pool size limits
- Implement connection validation

### Pattern 3: SSL Handshake Failures
**Cause:** Missing SSL parameters in connection string
**Fix:** Add `sslmode=require` (PostgreSQL) or `ssl=true` (MySQL)

### Pattern 4: Authentication Failures After Migration
**Cause:** User host restrictions or pg_hba.conf rules
**Fix:** Check user permissions and allowed hosts

## Connection String Templates

### PostgreSQL
```
postgresql://user:pass@host:5432/db?sslmode=require&connect_timeout=10
```

### MySQL
```
mysql://user:pass@host:3306/db?ssl=true&connectTimeout=10000
```

### With Connection Pool (Node.js)
```javascript
{
  host: 'endpoint',
  port: 5432,
  database: 'db',
  user: 'user',
  password: 'pass',
  ssl: { rejectUnauthorized: false },
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 10000
}
```

## Monitoring Checklist

- [ ] Connection count vs max_connections
- [ ] Active vs idle connections
- [ ] Connection wait time
- [ ] Query execution time
- [ ] Error rate by type

## Related
- [RDS Connectivity Guide](../aws/rds-connectivity-troubleshooting.md)
- RDS Proxy Connections
