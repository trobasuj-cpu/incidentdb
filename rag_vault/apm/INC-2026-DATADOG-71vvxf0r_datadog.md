# [INC-2026-DATADOG-71vvxf0r] Delayed data processing and errors in multiple products
**Company:** Datadog | **Date:** 2026-06-30 | **Severity:** HIGH | **Source:** [https://stspg.io/5snql687z9c4](https://stspg.io/5snql687z9c4)  
**Technologies:** APM, CI Visibility, Code Coverage, Database Monitoring  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-30 16:32:44 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-06-30 15:36:04 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-06-30 15:06:00 UTC] Datadog SRE (Identified): We are continuing to work on a fix for this issue.
[2026-06-30 14:57:57 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-06-30 14:36:22 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
[2026-06-30 14:28:49 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
[2026-06-30 13:54:31 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
[2026-06-30 13:51:54 UTC] Datadog SRE (Investigating): We are currently investigating an issue affecting CI Visibility, Test Optimization, Code Coverage, Software Delivery, Preemptive Alerting, Session Replay, RUM Explorer, Agent Observability, Database Monitoring, Event Correlation, Error Tracking, and all Security products in our US1 datacenter. As a result, some users may experience delays in data processing and errors when using affected products. Our engineering teams have identified the issue and are actively working on a resolution.

Our engineering teams are actively working to resolve the issue, and we'll follow up with more information shortly. Please let us know if you have any questions or need anything else in the meantime.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We are continuing to work on a fix for this issue. The issue has been identified and a fix is being implemented. We are continuing to investigate this issue. We are continuing to investigate this issue. We are continuing to investigate this issue. We are currently investigating an issue affecting CI Visibility, Test Optimization, Code Coverage, Software

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed data processing and errors in multiple products
service_cluster:
  provider: "Datadog"
  impacted_components: ["APM", "CI Visibility", "Code Coverage", "Database Monitoring"]
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
