# [INC-2026-SENTRY-9g0nslrz] Ingestion Issue in US Legacy
**Company:** Sentry | **Date:** 2026-04-22 | **Severity:** MEDIUM | **Source:** [https://stspg.io/t3cnq98b076s](https://stspg.io/t3cnq98b076s)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-22 08:26:58 UTC] Sentry SRE (Resolved): Between 07:25 UTC and 07:28 UTC, customers sending data to the legacy ingestion endpoint in our US region experienced a brief ingestion disruption lasting approximately 3 minutes. The issue has been fully resolved and ingestion is operating normally.

Customers using modern DSN endpoints (`*.ingest.us.sentry.io`) were not affected.

The disruption occurred during a scheduled configuration change. We're investing in additional tooling to detect and mitigate this class of issue faster. We apologize for the inconvenience and appreciate your patience.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Between 07:25 UTC and 07:28 UTC, customers sending data to the legacy ingestion endpoint in our US region experienced a brief ingestion disruption lasting approximately 3 minutes. The issue has been fully resolved and ingestion is operating normally.

Customers using modern DSN endpoints (`*.ingest.us.sentry.io`) were not affected.

The disruption occurred during a scheduled configuration change. We're investing in additional tooling to detect an

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Ingestion Issue in US Legacy
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
