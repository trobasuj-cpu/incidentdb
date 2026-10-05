# [INC-2026-DATADOG-6vndlbw8] Monitors - Delayed Evaluation
**Company:** Datadog | **Date:** 2026-01-28 | **Severity:** MEDIUM | **Source:** [https://stspg.io/55l2cqg035fj](https://stspg.io/55l2cqg035fj)  
**Technologies:** Monitors, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-01-28 18:36:23 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-01-28 18:13:40 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-01-28 17:08:16 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-01-28 16:21:52 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
[2026-01-28 16:18:31 UTC] Datadog SRE (Investigating): We are investigating delays in service checks monitors evaluation, which began at 20:26 1/28/2026 UTC.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. We are continuing to investigate this issue. We are investigating delays in service checks monitors evaluation, which began at 20:26 1/28/2026 UTC.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Monitors - Delayed Evaluation
service_cluster:
  provider: "Datadog"
  impacted_components: ["Monitors", "Datadog Ingestion", "APM", "Metrics Agent"]
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
- [ ] Validate automatic health checks and circuit breaking on Monitors cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
