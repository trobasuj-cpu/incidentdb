# [INC-2020-NETFLIX-01] Global Playback Degradation via Cassandra Major Compaction IOPS Storm
**Company:** Netflix | **Date:** 2020-11-12 | **Severity:** HIGH  
**Technologies:** Apache Cassandra, AWS EC2, EBS, Java  
**Categories:** STORAGE_SUBSYSTEM, IOPS_STARVATION, CASSANDRA_COMPACTION  

---

## 1. Symptoms & Observed Errors
```text
WARN  [CompactionExecutor:1] 2020-11-12 18:22:04,192 CompactionManager.java:492 - Compacting (large) [SSTableReader(path='/var/lib/cassandra/data/keyspace/users-0a1')]
ERROR [ReadStage-4] 2020-11-12 18:22:45,892 ReadCallback.java:122 - DigestMismatchException: Mismatch for key DecoratedKey
org.apache.cassandra.exceptions.ReadTimeoutException: Operation timed out - received only 1 responses from 3 required
AWS CloudWatch: EBS VolumeReadOps exceeded burst bucket; BurstBalance = 0%
```

## 2. Root Cause Analysis
A scheduled cron job triggered major compaction simultaneously across multiple Cassandra cluster nodes. The simultaneous read-write sequential disk throughput completely exhausted the AWS gp2 EBS burst IOPS credit balance. Disk I/O latency surged from 2ms to over 1,500ms, causing Cassandra read stages to drop queries and trigger read timeouts across the user authorization service.

## 3. Breaking Configuration / Problematic Code
```
# cassandra.yaml with unconstrained compaction throughput:
concurrent_compactors: 8
compaction_throughput_mb_per_sec: 0  # 0 indicates unthrottled disk saturation
concurrent_reads: 64
read_request_timeout_in_ms: 5000
```

## 4. Remediation Patch / Corrected Configuration
```
# Tuned compaction rate limiting and provisioned IOPS disk migration
concurrent_compactors: 2
compaction_throughput_mb_per_sec: 32  # Strict rate limit preserving I/O bandwidth for client reads
concurrent_reads: 32
read_request_timeout_in_ms: 10000
# Migrated EBS storage tier to io2 with provisioned 10,000 IOPS per volume
```

## 5. Prevention & Hardening Checklist
- [ ] Always enforce strict compaction_throughput_mb_per_sec limits on all distributed database storage tiers
- [ ] Monitor EBS VolumeBurstBalance metrics with high-priority alerting at 30% threshold
- [ ] Stagger compaction and administrative background operations using distributed token ring scheduling
