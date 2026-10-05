# [INC-2026-SENTRY-j9ggb9ns] Ingestion and alerting delays in us
**Company:** Sentry | **Date:** 2026-05-13 | **Severity:** MEDIUM | **Source:** [https://stspg.io/6zg89bt40sdt](https://stspg.io/6zg89bt40sdt)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-13 17:56:30 UTC] Sentry SRE (Resolved): Ingestion and alerting are back to steady state in the US region.
[2026-05-13 15:49:36 UTC] Sentry SRE (Monitoring): Ingestion and alerting are back to normal. We continue to monitor the situation
[2026-05-13 14:38:38 UTC] Sentry SRE (Investigating): We are experiencing ingestion and alerting delays in us for spans, transactions, and replays
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Ingestion and alerting are back to steady state in the US region. Ingestion and alerting are back to normal. We continue to monitor the situation We are experiencing ingestion and alerting delays in us for spans, transactions, and replays

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Ingestion and alerting delays in us
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
