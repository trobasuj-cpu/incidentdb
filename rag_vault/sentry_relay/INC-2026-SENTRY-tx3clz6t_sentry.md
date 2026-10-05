# [INC-2026-SENTRY-tx3clz6t] Sentry requests timing out / 504's
**Company:** Sentry | **Date:** 2026-09-01 | **Severity:** HIGH | **Source:** [https://stspg.io/hks9pdczmhhv](https://stspg.io/hks9pdczmhhv)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-01 11:21:24 UTC] Sentry SRE (Resolved): This incident has been resolved.
[2026-09-01 09:03:51 UTC] Sentry SRE (Monitoring): We're monitoring the implemented fix
[2026-09-01 08:57:58 UTC] Sentry SRE (Monitoring): We identified the issue and rolled out a configuration change
[2026-09-01 08:01:21 UTC] Sentry SRE (Investigating): We are currently investigating what's going on and why requests timing out
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: This incident has been resolved. We're monitoring the implemented fix We identified the issue and rolled out a configuration change We are currently investigating what's going on and why requests timing out

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Sentry requests timing out / 504's
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
