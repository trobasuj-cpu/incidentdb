# [INC-2019-STRIPE-01] Elevated API 500 Rates via Redis Sentinel Promotion Race Condition
**Company:** Stripe | **Date:** 2019-07-10 | **Severity:** HIGH  
**Technologies:** Redis, Redis Sentinel, Ruby, Linux  
**Categories:** CACHE_SUBSYSTEM, SPLIT_BRAIN, RACE_CONDITION  

---

## 1. Symptoms & Observed Errors
```text
READONLY You can't write against a read only replica.
Redis::CommandError: MOVED 12182 10.0.4.19:6379
Stripe-Error-Code: api_charge_failed (HTTP 500)
Rate of 5xx errors on /v1/charges surged to 18.4% globally.
```

## 2. Root Cause Analysis
During a routine primary node replacement in a Redis cluster, Sentinel nodes initiated automated failover. Due to an asymmetric network partition between Sentinel pods, two different replica instances were concurrently promoted to primary. Application workers routed write commands to an instance that the primary cluster topology considered read-only, causing charge authorization records to fail with READONLY exceptions.

## 3. Breaking Configuration / Problematic Code
```
# Sentinel configuration with low quorum and insufficient down-after-milliseconds:
sentinel monitor master-cluster 10.0.4.10 6379 2
sentinel down-after-milliseconds master-cluster 1000
sentinel failover-timeout master-cluster 2000
```

## 4. Remediation Patch / Corrected Configuration
```
# Tuned Sentinel quorum requiring majority consensus and longer heartbeat thresholds
sentinel monitor master-cluster 10.0.4.10 6379 3
sentinel down-after-milliseconds master-cluster 5000
sentinel failover-timeout master-cluster 15000
# Configured min-replicas-to-write on Redis primaries
min-replicas-to-write 1
min-replicas-max-lag 10
```

## 5. Prevention & Hardening Checklist
- [ ] Mandate min-replicas-to-write on all transactional Redis primaries to prevent writes on split-brain isolates
- [ ] Ensure Sentinel monitor quorum requires a strict mathematical majority (> N/2) of active monitor nodes
- [ ] Implement client-side connection topology caching with automatic backoff on MOVED/READONLY responses
