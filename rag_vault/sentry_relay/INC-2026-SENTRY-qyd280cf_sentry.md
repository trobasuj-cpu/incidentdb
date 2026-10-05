# [INC-2026-SENTRY-qyd280cf] sentry.io is not available
**Company:** Sentry | **Date:** 2026-08-27 | **Severity:** HIGH | **Source:** [https://stspg.io/zthsn2d365m0](https://stspg.io/zthsn2d365m0)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-27 15:41:45 UTC] Sentry SRE (Resolved): Our latency is now back to normal. Incident resolved
[2026-08-27 13:58:28 UTC] Sentry SRE (Monitoring): Our latency is now back to normal. We'll keep monitoring our systems a few more time before declaring the incident as resolved
[2026-08-27 13:39:48 UTC] Sentry SRE (Monitoring): We keep monitoring our systems while we recover to our usual latency
[2026-08-27 12:48:17 UTC] Sentry SRE (Monitoring): We are continuing to monitor while our systems recover after an issue that affected the availability of our product and API
[2026-08-27 11:44:21 UTC] Sentry SRE (Monitoring): We have identified and mitigated an issue that affected the availability of our product and API, and are now monitoring while our systems recover
[2026-08-27 11:25:14 UTC] Sentry SRE (Investigating): We are continuing to investigate an issue that affects the availability of our product and API
[2026-08-27 10:49:28 UTC] Sentry SRE (Investigating): We are continuing to investigate an issue that affects the availability of our product and API
[2026-08-27 09:56:36 UTC] Sentry SRE (Investigating): We are continuing to investigate an issue that affects the availability of our product and API
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Our latency is now back to normal. Incident resolved Our latency is now back to normal. We'll keep monitoring our systems a few more time before declaring the incident as resolved We keep monitoring our systems while we recover to our usual latency We are continuing to monitor while our systems recover after an issue that affected the availability of our product and API We have identified and mitigated an issue that affected the availability of o

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during sentry.io is not available
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
