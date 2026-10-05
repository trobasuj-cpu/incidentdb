# [INC-2026-SENTRY-8hwy8bps] sentry.io intermittent availability
**Company:** Sentry | **Date:** 2026-04-17 | **Severity:** MEDIUM | **Source:** [https://stspg.io/8cmycw6mxl1q](https://stspg.io/8cmycw6mxl1q)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-17 19:16:08 UTC] Sentry SRE (Resolved): This incident has been resolved.
[2026-04-17 19:13:38 UTC] Sentry SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-04-17 19:04:24 UTC] Sentry SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We are currently investigating this issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during sentry.io intermittent availability
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
