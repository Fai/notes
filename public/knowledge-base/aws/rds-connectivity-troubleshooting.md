---
tags: [aws, rds, database, postgresql, troubleshooting]
created: 2025-12-18
updated: 2025-12-18
---

# RDS Connectivity Troubleshooting Guide

Quick reference for diagnosing and resolving RDS connection issues.

## Quick Decision Tree

```
RDS Connection Issue
│
├─ Error: "Connection timeout" (ETIMEDOUT)
│   └─ Check: Security Groups, NACLs, Route Tables
│
├─ Error: "Connection refused" (ECONNREFUSED)  
│   └─ Check: RDS endpoint/port, instance status
│
├─ Error: "DNS not found" (ENOTFOUND)
│   └─ Check: VPC DNS settings, endpoint spelling
│
├─ Error: "User denied access" + Network works
│   └─ Check: User permissions, host restrictions, pg_hba.conf
│
├─ Error: "SSL required" / "pg_hba.conf entry"
│   └─ Check: DATABASE_URL SSL parameters (sslmode=require)
│
└─ Error: "Too many connections"
    └─ Check: Connection pool settings, connection leaks
```

## Common Issues by Frequency

### 1. SSL/TLS Configuration (Most Common)
```
# PostgreSQL connection string with SSL
postgresql://user:pass@host:5432/db?sslmode=require

# MySQL connection string with SSL
mysql://user:pass@host:3306/db?ssl=true
```

### 2. Security Group Rules
- Ensure inbound rule allows traffic from application security group
- Port: 5432 (PostgreSQL) or 3306 (MySQL)
- Source: Application security group ID (not CIDR for ECS/EKS)

### 3. User Authentication
```sql
-- Check user host restrictions (PostgreSQL)
SELECT usename, usesuper, usecreatedb FROM pg_user;

-- Check pg_hba.conf equivalent in RDS
-- Use Parameter Group: rds.force_ssl = 1
```

### 4. Connection Pool Settings
```
# Recommended pool sizes
Min Pool Size: 5
Max Pool Size: 20-50 (per container)
Connection Timeout: 30 seconds
Idle Timeout: 300 seconds
```

## RDS Proxy Considerations

When using RDS Proxy:
- MaxConnectionsPercent: Start with 50-70%, not 100%
- Connection multiplexing reduces actual DB connections
- Monitor `DatabaseConnectionsCurrentlySessionPinned` metric
- Check for connection leaks in application code

## Diagnostic Commands

```bash
# Test connectivity from ECS task
nc -zv <rds-endpoint> 5432

# Check DNS resolution
nslookup <rds-endpoint>

# Test with psql
psql "host=<endpoint> port=5432 dbname=<db> user=<user> sslmode=require"
```

## Related
- RDS Proxy Connection Analysis
- ECS to RDS Connectivity
