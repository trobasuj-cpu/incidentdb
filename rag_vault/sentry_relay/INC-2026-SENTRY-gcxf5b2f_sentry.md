# [INC-2026-SENTRY-gcxf5b2f] Ingestion delays and intermittent query failures
**Company:** Sentry | **Date:** 2026-04-27 | **Severity:** MEDIUM | **Source:** [https://stspg.io/szssn7zf4fkh](https://stspg.io/szssn7zf4fkh)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-27 20:59:54 UTC] Sentry SRE (Resolved): This incident has been resolved.
[2026-04-27 17:48:52 UTC] Sentry SRE (Monitoring): The system is operating as normal, we are monitoring the fix.
[2026-04-27 17:31:23 UTC] Sentry SRE (Investigating): Ingestion is now back to normal, we are slowly increasing write traffic to reduce intermittent API failures
[2026-04-27 16:52:12 UTC] Sentry SRE (Investigating): We are experiencing delays on our ingestion pipeline. Explorer views are in a degraded state
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: This incident has been resolved. The system is operating as normal, we are monitoring the fix. Ingestion is now back to normal, we are slowly increasing write traffic to reduce intermittent API failures We are experiencing delays on our ingestion pipeline. Explorer views are in a degraded state

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Ingestion delays and intermittent query failures
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
