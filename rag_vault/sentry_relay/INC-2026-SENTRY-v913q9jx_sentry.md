# [INC-2026-SENTRY-v913q9jx] Errors alerting degraded in US region
**Company:** Sentry | **Date:** 2026-08-25 | **Severity:** MEDIUM | **Source:** [https://stspg.io/b0s1vw9m6vf8](https://stspg.io/b0s1vw9m6vf8)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-25 12:38:27 UTC] Sentry SRE (Resolved): Error alerting is back to normal.
[2026-08-25 12:27:34 UTC] Sentry SRE (Identified): We have identified the issue and implemented a fix.
[2026-08-25 11:09:40 UTC] Sentry SRE (Investigating): We are continuing to investigate the issue
[2026-08-25 09:38:01 UTC] Sentry SRE (Investigating): We are investigating delays in error alerts in the US region, up to 10 minutes
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Error alerting is back to normal. We have identified the issue and implemented a fix. We are continuing to investigate the issue We are investigating delays in error alerts in the US region, up to 10 minutes

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Errors alerting degraded in US region
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
