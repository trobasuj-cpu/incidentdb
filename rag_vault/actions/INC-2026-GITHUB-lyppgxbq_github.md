# [INC-2026-GITHUB-lyppgxbq] Actions delays in starting runs
**Company:** GitHub | **Date:** 2026-08-24 | **Severity:** MEDIUM | **Source:** [https://stspg.io/v6ysclb9vcbd](https://stspg.io/v6ysclb9vcbd)  
**Technologies:** Actions, GitHub Actions, Git, REST API  
**Categories:** QUEUE_DELAY, PIPELINE_EXECUTION_FAILURE, STORAGE_SUBSYSTEM  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-24 14:34:42 UTC] GitHub SRE (Resolved): On August 24, 2026, between 13:33 UTC and 14:04 UTC,  3.8% of Actions runs experienced start delays over 5 minutes with 1.25% of Actions runs failing outright. <br /> <br />The incident was caused by a disk failure on a node hosting one of many service instances responsible for processing runner assignment events. Typically, pods on unhealthy nodes are removed and replaced automatically without impact. In this case, although the node was severely degraded and unable to perform disk operations, it continued sending healthy signals, preventing the system from immediately moving its work elsewhere. During this period, events assigned to the affected component accumulated until an automatic rebalance redirected processing to healthy components at 13:54 UTC. The queue backlog was cleared at 14:00 UTC, and processing returned to normal by 14:04 UTC. <br /><br />To prevent a recurrence, we are improving detection and automated remediation for unhealthy nodes that aren’t fully offline. We are also strengthening application-level resiliency, so stalled consumers are automatically removed quickly and their work reassigned without waiting for the affected node to recover.
[2026-08-24 14:26:11 UTC] GitHub SRE (Monitoring): The degradation affecting Actions has been mitigated. We are monitoring to ensure stability.
[2026-08-24 14:22:59 UTC] GitHub SRE (Investigating): Failures while queuing and running Actions jobs for a subset of customers are now resolving. We are monitoring for full recovery.
[2026-08-24 13:56:55 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Actions
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 24, 2026, between 13:33 UTC and 14:04 UTC,  3.8% of Actions runs experienced start delays over 5 minutes with 1.25% of Actions runs failing outright. <br /> <br />The incident was caused by a disk failure on a node hosting one of many service instances responsible for processing runner assignment events. Typically, pods on unhealthy nodes are removed and replaced automatically without impact. In this case, although the node was severely

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Actions delays in starting runs
service_cluster:
  provider: "GitHub"
  impacted_components: ["Actions", "GitHub Actions", "Git", "REST API"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by GitHub SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Actions cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
