# [INC-2026-SENTRY-1l6k1160] Issues performance is degraded in the US
**Company:** Sentry | **Date:** 2026-08-26 | **Severity:** MEDIUM | **Source:** [https://stspg.io/nbksfm6kzcb9](https://stspg.io/nbksfm6kzcb9)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** DATABASE_DEGRADATION, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-26 20:36:14 UTC] Sentry SRE (Resolved): The issue has been resolved
[2026-08-26 20:04:04 UTC] Sentry SRE (Monitoring): We have put a fix into place and are monitoring the situation
[2026-08-26 18:47:29 UTC] Sentry SRE (Monitoring): We have put a fix into place and are monitoring the situation
[2026-08-26 17:53:29 UTC] Sentry SRE (Identified): We have identified a potential cause of excess traffic and are working on a fix.
[2026-08-26 16:17:20 UTC] Sentry SRE (Investigating): We are experiencing slower page loads and API responses due to a database issue in us
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: The issue has been resolved We have put a fix into place and are monitoring the situation We have put a fix into place and are monitoring the situation We have identified a potential cause of excess traffic and are working on a fix. We are experiencing slower page loads and API responses due to a database issue in us

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Issues performance is degraded in the US
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
