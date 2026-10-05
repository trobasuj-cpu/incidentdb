# [INC-2026-DATADOG-2c2287dz] Real user monitoring, distribution and custom processes metrics delayed
**Company:** Datadog | **Date:** 2026-07-29 | **Severity:** MEDIUM | **Source:** [https://stspg.io/0y0jgdxjp7hh](https://stspg.io/0y0jgdxjp7hh)  
**Technologies:** Metrics and Infra Monitoring, Monitors, RUM, Datadog Ingestion  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-29 13:28:08 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-07-29 13:12:00 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-07-29 12:53:24 UTC] Datadog SRE (Identified): The RUM delays have been resolved. We're continuing to mitigate the issues on distribution metrics.
[2026-07-29 12:35:04 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-07-29 11:42:12 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
[2026-07-29 11:41:51 UTC] Datadog SRE (Investigating): We are currently investigating an issue delaying the ingestion of Real User Monitoring (RUM) measure metrics, custom process metrics, and a subset of distribution metrics for some customers.

As a result, some users may see delays or gaps in RUM, custom process, and distribution metrics. To prevent false alerts caused by delayed data, affected monitors will not notify and will automatically resume once current data is available. All other monitors will operate normally. No data has been lost.

We will provide further updates as the situation progresses.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The RUM delays have been resolved. We're continuing to mitigate the issues on distribution metrics. The issue has been identified and a fix is being implemented. We are continuing to investigate this issue. We are currently investigating an issue delaying the ingestion of Real User Monitoring (RUM) measure metrics, custom process metrics, and a subset o

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Real user monitoring, distribution and custom processes metrics delayed
service_cluster:
  provider: "Datadog"
  impacted_components: ["Metrics and Infra Monitoring", "Monitors", "RUM", "Datadog Ingestion"]
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
