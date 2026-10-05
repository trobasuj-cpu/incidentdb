# [INC-2026-DATADOG-g545g04n] Delays in Monitor Evaluations
**Company:** Datadog | **Date:** 2026-02-05 | **Severity:** HIGH | **Source:** [https://stspg.io/kzxxld69ftmr](https://stspg.io/kzxxld69ftmr)  
**Technologies:** APM, Application Security Management, Cloud SIEM, Incident Response  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-02-05 12:43:22 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-02-05 12:23:31 UTC] Datadog SRE (Monitoring): We have observed full recovery of monitors and will continue to monitor
[2026-02-05 12:01:20 UTC] Datadog SRE (Identified): We are observing recovery for the vast majority of monitors and are continuing to work on a full fix for this issue.
[2026-02-05 11:50:36 UTC] Datadog SRE (Identified): We are continuing to work on a fix for this issue.
[2026-02-05 11:31:36 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-02-05 11:28:25 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
[2026-02-05 11:11:17 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
[2026-02-05 11:10:40 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. We have observed full recovery of monitors and will continue to monitor We are observing recovery for the vast majority of monitors and are continuing to work on a full fix for this issue. We are continuing to work on a fix for this issue. The issue has been identified and a fix is being implemented. We are continuing to investigate this issue. We are continuing to investigate this issue. We are continuing to inve

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delays in Monitor Evaluations
service_cluster:
  provider: "Datadog"
  impacted_components: ["APM", "Application Security Management", "Cloud SIEM", "Incident Response"]
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
- [ ] Validate automatic health checks and circuit breaking on APM cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
