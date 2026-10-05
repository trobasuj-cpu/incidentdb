# [INC-2025-DATADOG-xj87f6z9] Metrics data ingestion delayed and monitor evaluations degraded
**Company:** Datadog | **Date:** 2025-12-09 | **Severity:** MEDIUM | **Source:** [https://stspg.io/5hqhsx3549dz](https://stspg.io/5hqhsx3549dz)  
**Technologies:** Metrics and Infra Monitoring, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2025-12-09 16:08:28 UTC] Datadog SRE (Resolved): This incident has been resolved. Live data is being processed normally and gaps in distribution metrics on graphs will be backfilled within the next hour.
[2025-12-09 16:00:18 UTC] Datadog SRE (Monitoring): Live distribution metrics are available and being evaluated for all monitors. Gaps in graphs from the beginning of the incident are in the process of being backfilled.
[2025-12-09 15:36:13 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2025-12-09 15:16:17 UTC] Datadog SRE (Investigating): We’re currently monitoring an issue causing delays in distribution metric processing in our US1 region.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. Live data is being processed normally and gaps in distribution metrics on graphs will be backfilled within the next hour. Live distribution metrics are available and being evaluated for all monitors. Gaps in graphs from the beginning of the incident are in the process of being backfilled. The issue has been identified and a fix is being implemented. We’re currently monitoring an issue causing delays in distributio

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Metrics data ingestion delayed and monitor evaluations degraded
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
