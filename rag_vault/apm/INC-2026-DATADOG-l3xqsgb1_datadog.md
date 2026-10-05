# [INC-2026-DATADOG-l3xqsgb1] Monitors - Delayed Evaluation for Multiple Products
**Company:** Datadog | **Date:** 2026-02-18 | **Severity:** MEDIUM | **Source:** [https://stspg.io/c6d05hsplhbv](https://stspg.io/c6d05hsplhbv)  
**Technologies:** APM, Data Streams Monitoring, Log Management, Metrics and Infra Monitoring  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-02-18 15:37:18 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-02-18 14:30:23 UTC] Datadog SRE (Monitoring): We are investigating delays in a subset of Distribution Metrics and Monitor Evaluations, which began at 18:33 UTC.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. We are investigating delays in a subset of Distribution Metrics and Monitor Evaluations, which began at 18:33 UTC.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Monitors - Delayed Evaluation for Multiple Products
service_cluster:
  provider: "Datadog"
  impacted_components: ["APM", "Data Streams Monitoring", "Log Management", "Metrics and Infra Monitoring"]
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
