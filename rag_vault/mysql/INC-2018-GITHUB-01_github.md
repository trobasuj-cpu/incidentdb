# [INC-2018-GITHUB-01] Cross-Datacenter Network Glitch Resulting in Split-Brain MySQL State Inconsistency
**Company:** GitHub | **Date:** 2018-10-21 | **Severity:** CRITICAL  
**Technologies:** MySQL, Orchestrator, HAProxy, Consul  
**Categories:** SPLIT_BRAIN, NETWORK_PARTITION, DATABASE, DATA_INTEGRITY  

---

## 1. Symptoms & Observed Errors
```text
[ERROR] Failed to connect to MySQL master at dc1-db-01: Connection timed out
[INFO] Orchestrator: Promoting dc2-db-01 to master in secondary datacenter
[WARN] Split-brain detected: Multiple active masters accepting writes
MySQL Error 1062: Duplicate entry '1049281' for key 'PRIMARY'
InnoDB: Inconsistent GTID set executed across replicas
```

## 2. Root Cause Analysis
During scheduled maintenance of an optical fiber link between US-East and US-West datacenters, a brief 43-second packet drop severed connection between orchestrator nodes and the primary database. Automated failover software elected a replica in US-East as the new master while the existing master in US-West continued receiving writes from local web application tiers. When connectivity resumed, both databases contained conflicting GTIDs and primary key collisions, requiring 24 hours of manual data surgery.

## 3. Breaking Configuration / Problematic Code
```
# Orchestrator failover configuration lacking fencing (STONITH):
{
  "RecoveryThreshold": 1,
  "FailoverMethod": "fastest_replica",
  "EnableSemiSyncEnforcement": false,
  "EnforceSingleMasterConsensus": false
}
```

## 4. Remediation Patch / Corrected Configuration
```
# Production orchestrator configuration with strict quorum & Raft fencing
{
  "RecoveryThreshold": 3,
  "RaftEnabled": true,
  "RaftNodes": ["dc1-orch-01", "dc1-orch-02", "dc2-orch-01"],
  "FailoverMethod": "semi_sync_consensus",
  "EnableSemiSyncEnforcement": true,
  "PreFailoverProcesses": ["/usr/local/bin/fence_old_master.sh"],
  "PostFailoverProcesses": ["/usr/local/bin/update_consul_routing.sh"]
}
```

## 5. Prevention & Hardening Checklist
- [ ] Enforce Raft/Paxos consensus quorum before promoting any database replica to writable master
- [ ] Implement STONITH (Shoot The Other Node In The Head) fencing to immediately cut power/network to partitioned primary
- [ ] Mandate semi-synchronous replication with wait-for-ack across physical geographic failure boundaries
