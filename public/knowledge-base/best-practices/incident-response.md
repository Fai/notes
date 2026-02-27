---
title: "Incident Response Best Practices"
tags: [best-practices, incident-response, operations, runbook]
created: 2025-12-18
updated: 2025-12-18
---
title: "Incident Response"

# Incident Response Best Practices

Structured approach to handling production incidents.

## Incident Severity Levels

| Level | Description | Response Time | Examples |
|-------|-------------|---------------|----------|
| P1 | Service down | 15 min | Complete outage, data loss |
| P2 | Major degradation | 30 min | Partial outage, slow response |
| P3 | Minor impact | 4 hours | Non-critical feature broken |
| P4 | Low impact | 24 hours | Cosmetic issues |

## Response Workflow

### 1. Acknowledge (First 5 minutes)
- [ ] Acknowledge the incident
- [ ] Assess initial severity
- [ ] Start incident channel/thread
- [ ] Notify stakeholders

### 2. Triage (5-15 minutes)
- [ ] Identify affected services
- [ ] Check recent deployments
- [ ] Review monitoring dashboards
- [ ] Gather initial evidence

### 3. Mitigate (15-60 minutes)
- [ ] Implement quick fix or rollback
- [ ] Scale resources if needed
- [ ] Enable additional logging
- [ ] Communicate status updates

### 4. Resolve
- [ ] Implement permanent fix
- [ ] Verify fix in production
- [ ] Update documentation
- [ ] Close incident

### 5. Post-Mortem (Within 48 hours)
- [ ] Document timeline
- [ ] Identify root cause
- [ ] List action items
- [ ] Share learnings

## Quick Diagnostic Commands

### Kubernetes/EKS
```bash
# Check pod status
kubectl get pods -A | grep -v Running

# Recent events
kubectl get events --sort-by='.lastTimestamp' | tail -20

# Pod logs
kubectl logs -f <pod> --previous

# Resource usage
kubectl top pods -A
```

### AWS
```bash
# Recent CloudWatch alarms
aws cloudwatch describe-alarms --state-value ALARM

# ECS service events
aws ecs describe-services --cluster <cluster> --services <service> \
  --query 'services[].events[:5]'

# RDS events
aws rds describe-events --duration 60
```

### Database
```sql
-- Active queries (PostgreSQL)
SELECT pid, now() - pg_stat_activity.query_start AS duration, query
FROM pg_stat_activity
WHERE state = 'active' AND query NOT LIKE '%pg_stat_activity%'
ORDER BY duration DESC;

-- Lock waits
SELECT * FROM pg_locks WHERE NOT granted;
```

## Communication Templates

### Initial Notification
```
🔴 INCIDENT: [Brief description]
Severity: P[1-4]
Impact: [Who/what is affected]
Status: Investigating
Next update: [Time]
```

### Status Update
```
🟡 UPDATE: [Incident name]
Status: [Investigating/Mitigating/Resolved]
Progress: [What's been done]
ETA: [If known]
Next update: [Time]
```

### Resolution
```
🟢 RESOLVED: [Incident name]
Duration: [Start to end time]
Root cause: [Brief explanation]
Post-mortem: [Link or scheduled date]
```

## Post-Mortem Template

```markdown
# Incident Post-Mortem: [Title]

## Summary
- Date: 
- Duration: 
- Severity: 
- Impact: 

## Timeline
| Time | Event |
|------|-------|
| HH:MM | ... |

## Root Cause
[Detailed explanation]

## Resolution
[What fixed it]

## Action Items
- [ ] [Action] - Owner - Due date

## Lessons Learned
- What went well
- What could improve
```

## Related
- [Monitoring Setup](./monitoring-setup.md)
- Runbook Template
