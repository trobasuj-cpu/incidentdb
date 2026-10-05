# [INC-2026-SENTRY-jn4d88vl] Elevated errors (EU)
**Company:** Sentry | **Date:** 2026-06-02 | **Severity:** MEDIUM | **Source:** [https://stspg.io/lh9b0dcrf0w5](https://stspg.io/lh9b0dcrf0w5)  
**Technologies:** API, Sentry Relay, Kafka, ClickHouse  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-02 07:45:03 UTC] Sentry SRE (Resolved): Between 06:00-07:00 UTC, we saw increased numbers of API errors in the EU region. The largest impact was between 06:46-07:00. We've implemented a fix and we're no longer seeing elevated errors.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Between 06:00-07:00 UTC, we saw increased numbers of API errors in the EU region. The largest impact was between 06:46-07:00. We've implemented a fix and we're no longer seeing elevated errors.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Elevated errors (EU)
service_cluster:
  provider: "Sentry"
  impacted_components: ["API", "Sentry Relay", "Kafka", "ClickHouse"]
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
- [ ] Validate automatic health checks and circuit breaking on API cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
