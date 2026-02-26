---
tags: [best-practices, migration, checklist, operations]
created: 2025-12-18
updated: 2025-12-18
---

# Migration Checklist

Universal checklist for infrastructure and application migrations.

## Pre-Migration

### Planning
- [ ] Document current state architecture
- [ ] Define success criteria
- [ ] Identify dependencies
- [ ] Estimate downtime window
- [ ] Create rollback plan
- [ ] Schedule maintenance window
- [ ] Notify stakeholders

### Preparation
- [ ] Backup all data and configurations
- [ ] Test migration in non-production
- [ ] Prepare monitoring dashboards
- [ ] Document step-by-step runbook
- [ ] Assign roles (executor, verifier, communicator)

## During Migration

### Execution
- [ ] Start incident channel for communication
- [ ] Follow runbook step-by-step
- [ ] Verify each step before proceeding
- [ ] Document any deviations
- [ ] Monitor system metrics

### Verification
- [ ] Health checks pass
- [ ] Smoke tests pass
- [ ] Performance baseline met
- [ ] No error spikes in logs
- [ ] Data integrity verified

## Post-Migration

### Immediate (First hour)
- [ ] Monitor error rates
- [ ] Check response times
- [ ] Verify all integrations
- [ ] Confirm data consistency

### Short-term (24-48 hours)
- [ ] Review logs for anomalies
- [ ] Compare metrics to baseline
- [ ] Gather user feedback
- [ ] Document lessons learned

### Cleanup
- [ ] Remove old resources (after validation period)
- [ ] Update documentation
- [ ] Archive migration artifacts
- [ ] Close migration ticket

## Rollback Triggers

Initiate rollback if:
- Error rate > 5% (or 2x baseline)
- Response time > 2x baseline
- Critical functionality broken
- Data corruption detected
- Migration exceeds time window

## Common Migration Types

### Database Migration
```
Pre:
- [ ] Verify backup
- [ ] Test restore procedure
- [ ] Check disk space
- [ ] Plan for connection string updates

During:
- [ ] Stop writes (if needed)
- [ ] Perform migration
- [ ] Verify data counts
- [ ] Update connection strings
- [ ] Resume traffic

Post:
- [ ] Monitor query performance
- [ ] Check replication lag
- [ ] Verify indexes
```

### Kubernetes Node Migration
```
Pre:
- [ ] Verify PodDisruptionBudgets
- [ ] Check node capacity
- [ ] Review pod anti-affinity

During:
- [ ] Cordon old nodes
- [ ] Drain pods gracefully
- [ ] Verify pods rescheduled
- [ ] Delete old nodes

Post:
- [ ] Verify all pods running
- [ ] Check persistent volumes
- [ ] Monitor resource usage
```

### Storage Migration (EBS gp2 → gp3)
```
Pre:
- [ ] Snapshot existing volumes
- [ ] Calculate IOPS/throughput needs
- [ ] Plan for volume modification time

During:
- [ ] Modify volume type
- [ ] Wait for optimization
- [ ] Verify performance

Post:
- [ ] Compare IOPS metrics
- [ ] Verify cost savings
- [ ] Delete old snapshots (after validation)
```

## Related
- [Incident Response](./incident-response.md)
- Runbook Template
