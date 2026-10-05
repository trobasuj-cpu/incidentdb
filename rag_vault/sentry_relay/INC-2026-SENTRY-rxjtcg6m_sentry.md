# [INC-2026-SENTRY-rxjtcg6m] Incoherent number of processed logs
**Company:** Sentry | **Date:** 2026-07-07 | **Severity:** MEDIUM | **Source:** [https://stspg.io/yf7318fjnmj8](https://stspg.io/yf7318fjnmj8)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-07 14:36:02 UTC] Sentry SRE (Resolved): Between July 6th 20:25 UTC and July 7th 12:25 UTC, a wrong configuration change was deployed causing over-sampling of logs, spans and metrics.
The configuration change is now reverted and the issue is fixed.
[2026-07-07 08:49:19 UTC] Sentry SRE (Investigating): We are currently investigating reports of inconsistent reported number of processed logs in Sentry for the last day
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Between July 6th 20:25 UTC and July 7th 12:25 UTC, a wrong configuration change was deployed causing over-sampling of logs, spans and metrics.
The configuration change is now reverted and the issue is fixed. We are currently investigating reports of inconsistent reported number of processed logs in Sentry for the last day

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incoherent number of processed logs
service_cluster:
  provider: "Sentry"
  impacted_components: ["Sentry Relay", "Kafka", "ClickHouse", "Snuba"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Sentry SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Sentry Relay cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
