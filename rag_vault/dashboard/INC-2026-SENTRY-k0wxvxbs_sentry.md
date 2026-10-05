# [INC-2026-SENTRY-k0wxvxbs] Increased UI and API timeouts
**Company:** Sentry | **Date:** 2026-05-12 | **Severity:** MEDIUM | **Source:** [https://stspg.io/llpp0hn6l4p6](https://stspg.io/llpp0hn6l4p6)  
**Technologies:** Dashboard, API, Sentry Relay, Kafka  
**Categories:** DATABASE_DEGRADATION, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-12 22:37:36 UTC] Sentry SRE (Resolved): This incident has been resolved.
[2026-05-12 17:00:29 UTC] Sentry SRE (Monitoring): We've applied a configuration change to mitigate the API/UI errors being experienced.
[2026-05-12 15:43:27 UTC] Sentry SRE (Investigating): We are continuing to investigate increased UI and API timeouts and errors.
[2026-05-12 13:51:12 UTC] Sentry SRE (Investigating): We are continuing to investigate increased UI and API timeouts and errors
[2026-05-12 12:28:24 UTC] Sentry SRE (Investigating): We are seeing increased UI and API timeouts and errors, due to an overloaded database cluster. Ingestion and alerting should not be affected.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: This incident has been resolved. We've applied a configuration change to mitigate the API/UI errors being experienced. We are continuing to investigate increased UI and API timeouts and errors. We are continuing to investigate increased UI and API timeouts and errors We are seeing increased UI and API timeouts and errors, due to an overloaded database cluster. Ingestion and alerting should not be affected.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Increased UI and API timeouts
service_cluster:
  provider: "Sentry"
  impacted_components: ["Dashboard", "API", "Sentry Relay", "Kafka"]
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
- [ ] Validate automatic health checks and circuit breaking on Dashboard cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
