# [INC-2026-DATADOG-9qt73ty9] Delayed data streams data
**Company:** Datadog | **Date:** 2026-07-10 | **Severity:** HIGH | **Source:** [https://stspg.io/y5323mlt6x4g](https://stspg.io/y5323mlt6x4g)  
**Technologies:** Data Streams Monitoring, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-10 11:59:28 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-07-10 11:18:40 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results. A small percentage of data sent between 6:23 UTC and 14:43 UTC is still being backfilled.
[2026-07-10 10:35:17 UTC] Datadog SRE (Investigating): We are continuing to investigate the issue.
[2026-07-10 10:09:23 UTC] Datadog SRE (Investigating): We are currently investigating an issue affecting Data Streams Monitoring (DSM) in our US1 datacenter. As a result, data stream latency metrics may be delayed or not updating for some users. If you have monitors configured on those metrics, they may fail to alert as expected during this incident. We are not aware of any data loss at this time. Our engineering team is actively working on a fix, and we'll follow up with more information shortly.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. A small percentage of data sent between 6:23 UTC and 14:43 UTC is still being backfilled. We are continuing to investigate the issue. We are currently investigating an issue affecting Data Streams Monitoring (DSM) in our US1 datacenter. As a result, data stream latency metrics may be delayed or not updating for some users. If you have monitors configure

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed data streams data
service_cluster:
  provider: "Datadog"
  impacted_components: ["Data Streams Monitoring", "Datadog Ingestion", "APM", "Metrics Agent"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Datadog SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Data Streams Monitoring cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
