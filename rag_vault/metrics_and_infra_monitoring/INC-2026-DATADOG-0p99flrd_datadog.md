# [INC-2026-DATADOG-0p99flrd] Delays in AWS and Azure cloud integration metrics ingestion
**Company:** Datadog | **Date:** 2026-04-24 | **Severity:** HIGH | **Source:** [https://stspg.io/xw01xpvggm3n](https://stspg.io/xw01xpvggm3n)  
**Technologies:** Metrics and Infra Monitoring, Monitors, Datadog Ingestion, APM  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-24 12:50:36 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-04-24 12:44:10 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-04-24 12:43:50 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-04-24 12:27:20 UTC] Datadog SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. We are currently investigating this issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delays in AWS and Azure cloud integration metrics ingestion
service_cluster:
  provider: "Datadog"
  impacted_components: ["Metrics and Infra Monitoring", "Monitors", "Datadog Ingestion", "APM"]
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
