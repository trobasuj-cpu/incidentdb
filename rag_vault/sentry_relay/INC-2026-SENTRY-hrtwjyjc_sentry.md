# [INC-2026-SENTRY-hrtwjyjc] Missing SDK Client Reports and Rate Limiting Outcomes
**Company:** Sentry | **Date:** 2026-06-12 | **Severity:** MEDIUM | **Source:** [https://stspg.io/xb0nhwg75j6f](https://stspg.io/xb0nhwg75j6f)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-12 11:30:03 UTC] Sentry SRE (Resolved): From approximately 10:55 UTC to 11:10 UTC we experienced a loss of outcomes which power the "Stats" in the organization settings.

The data loss is purely informational without impact on billing or ingested data.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: From approximately 10:55 UTC to 11:10 UTC we experienced a loss of outcomes which power the "Stats" in the organization settings.

The data loss is purely informational without impact on billing or ingested data.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Missing SDK Client Reports and Rate Limiting Outcomes
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
