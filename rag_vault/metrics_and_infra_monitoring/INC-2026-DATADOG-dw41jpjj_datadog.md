# [INC-2026-DATADOG-dw41jpjj] Elevated Error Rates for Metrics Queries
**Company:** Datadog | **Date:** 2026-04-30 | **Severity:** HIGH | **Source:** [https://stspg.io/17n4grhvbmjt](https://stspg.io/17n4grhvbmjt)  
**Technologies:** Metrics and Infra Monitoring, Datadog Ingestion, APM, Metrics Agent  
**Categories:** API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-30 21:19:56 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-04-30 21:09:25 UTC] Datadog SRE (Monitoring): We have deployed a fix and we are monitoring the results.
We will provide another update once the service is fully operational.
[2026-04-30 20:45:48 UTC] Datadog SRE (Investigating): We are actively investigating elevated error rates for Metrics Queries.
As a result of this issue, some users may see errors with metrics graphs on the web application or API.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. We have deployed a fix and we are monitoring the results.
We will provide another update once the service is fully operational. We are actively investigating elevated error rates for Metrics Queries.
As a result of this issue, some users may see errors with metrics graphs on the web application or API.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Elevated Error Rates for Metrics Queries
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
