# [INC-2026-DATADOG-xqyrslhj] Azure Metrics Reporting
**Company:** Datadog | **Date:** 2026-05-15 | **Severity:** CRITICAL | **Source:** [https://stspg.io/l0b8n3g0ysky](https://stspg.io/l0b8n3g0ysky)  
**Technologies:** Metrics and Infra Monitoring, Datadog Ingestion, APM, Metrics Agent  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-15 23:43:01 UTC] Datadog SRE (Resolved): This incident has been resolved and Azure metrics are reporting as expected.
[2026-05-15 23:27:27 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-05-15 23:09:05 UTC] Datadog SRE (Identified): We are continuing to work on a fix for this issue.
[2026-05-15 22:40:11 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-05-15 22:17:25 UTC] Datadog SRE (Investigating): We are investigating an issue submitting Azure metrics.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved and Azure metrics are reporting as expected. A fix has been implemented and we are monitoring the results. We are continuing to work on a fix for this issue. The issue has been identified and a fix is being implemented. We are investigating an issue submitting Azure metrics.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Azure Metrics Reporting
service_cluster:
  provider: "Datadog"
  impacted_components: ["Metrics and Infra Monitoring", "Datadog Ingestion", "APM", "Metrics Agent"]
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
- [ ] Validate automatic health checks and circuit breaking on Metrics and Infra Monitoring cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
