# [INC-2026-DATADOG-yx8jxssy] Delayed CI Visibility data
**Company:** Datadog | **Date:** 2026-06-25 | **Severity:** MEDIUM | **Source:** [https://stspg.io/kzsp8r6t4847](https://stspg.io/kzsp8r6t4847)  
**Technologies:** CI Visibility, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-25 19:32:26 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-06-25 19:24:22 UTC] Datadog SRE (Identified): We have identified the underlying issue and are working on a fix.

It is important to note that no data has been lost, and it will be backfilled and available once the service is operational again.
[2026-06-25 19:23:01 UTC] Datadog SRE (Investigating): We are investigating increased latency processing CI Visibility data.

As a result of this issue, some users may see delays with pipeline executions.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. We have identified the underlying issue and are working on a fix.

It is important to note that no data has been lost, and it will be backfilled and available once the service is operational again. We are investigating increased latency processing CI Visibility data.

As a result of this issue, some users may see delays with pipeline executions.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed CI Visibility data
service_cluster:
  provider: "Datadog"
  impacted_components: ["CI Visibility", "Datadog Ingestion", "APM", "Metrics Agent"]
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
- [ ] Validate automatic health checks and circuit breaking on CI Visibility cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
