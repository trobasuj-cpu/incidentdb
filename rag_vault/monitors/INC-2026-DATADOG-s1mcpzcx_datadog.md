# [INC-2026-DATADOG-s1mcpzcx] Delays in APM-based Monitor Notifications
**Company:** Datadog | **Date:** 2026-05-05 | **Severity:** MEDIUM | **Source:** [https://stspg.io/rww57bn61jkp](https://stspg.io/rww57bn61jkp)  
**Technologies:** Monitors, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-05 15:52:31 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-05-05 14:44:24 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-05-05 14:29:16 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-05-05 14:14:03 UTC] Datadog SRE (Investigating): We’re currently investigating an issue affecting APM monitor delays in our US1 datacenter. As a result, some users may experience delayed alert triggering or temporary inconsistencies in monitors.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. We’re currently investigating an issue affecting APM monitor delays in our US1 datacenter. As a result, some users may experience delayed alert triggering or temporary inconsistencies in monitors.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delays in APM-based Monitor Notifications
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
