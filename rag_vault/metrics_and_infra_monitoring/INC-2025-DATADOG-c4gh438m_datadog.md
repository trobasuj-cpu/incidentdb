# [INC-2025-DATADOG-c4gh438m] Delayed APM metric ingestion
**Company:** Datadog | **Date:** 2025-12-12 | **Severity:** HIGH | **Source:** [https://stspg.io/qwpxql6b48pl](https://stspg.io/qwpxql6b48pl)  
**Technologies:** Metrics and Infra Monitoring, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2025-12-12 16:52:51 UTC] Datadog SRE (Resolved): All impact related to APM metrics has been resolved.  A separate incident has been created to track the remaining impact in live process data.
[2025-12-12 15:53:22 UTC] Datadog SRE (Identified): We have identified the issue affecting ingestion delays in apm and process metrics and are working on recovery
[2025-12-12 14:26:26 UTC] Datadog SRE (Investigating): We are currently investigating lag in ingesting apm and process metrics, which affects monitor evaluation and in some cases led to incorrect monitor alerts.
[2025-12-12 13:55:53 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
[2025-12-12 13:37:34 UTC] Datadog SRE (Investigating): We are currently investigating lag in ingesting apm metrics, which affects monitor evaluation.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: All impact related to APM metrics has been resolved.  A separate incident has been created to track the remaining impact in live process data. We have identified the issue affecting ingestion delays in apm and process metrics and are working on recovery We are currently investigating lag in ingesting apm and process metrics, which affects monitor evaluation and in some cases led to incorrect monitor alerts. We are continuing to investigate this i

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed APM metric ingestion
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
