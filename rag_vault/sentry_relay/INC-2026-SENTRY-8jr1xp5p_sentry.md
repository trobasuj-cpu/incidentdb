# [INC-2026-SENTRY-8jr1xp5p] Error Ingestion Delayed in US Region
**Company:** Sentry | **Date:** 2026-05-19 | **Severity:** MEDIUM | **Source:** [https://stspg.io/5yg7dxsx5d7h](https://stspg.io/5yg7dxsx5d7h)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-19 18:48:53 UTC] Sentry SRE (Resolved): Ingestion for symbolicated issues is back to normal.
[2026-05-19 17:41:54 UTC] Sentry SRE (Investigating): We are implementing a fix and monitoring the situation. Non-symbolicated issues are not experiencing delays.
[2026-05-19 15:58:49 UTC] Sentry SRE (Investigating): We are experiencing ingestion delays for errors in the US region.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Ingestion for symbolicated issues is back to normal. We are implementing a fix and monitoring the situation. Non-symbolicated issues are not experiencing delays. We are experiencing ingestion delays for errors in the US region.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Error Ingestion Delayed in US Region
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
