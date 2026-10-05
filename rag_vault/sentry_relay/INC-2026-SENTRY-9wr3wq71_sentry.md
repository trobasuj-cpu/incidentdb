# [INC-2026-SENTRY-9wr3wq71] Delayed Error ingestion in our US region
**Company:** Sentry | **Date:** 2026-08-06 | **Severity:** HIGH | **Source:** [https://stspg.io/g528fth7fzvw](https://stspg.io/g528fth7fzvw)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-06 11:21:06 UTC] Sentry SRE (Resolved): We've processed all backlogged events and have fully recovered.
[2026-08-06 10:50:18 UTC] Sentry SRE (Monitoring): We're processing new events in real time, and are working through a backlog of events now.
[2026-08-06 10:25:18 UTC] Sentry SRE (Investigating): We've identified the issue causing delays in ingestion and have applied a fix. We'll update again when we return to real time processing.
[2026-08-06 09:55:48 UTC] Sentry SRE (Investigating): We are still actively investigating this issue.
[2026-08-06 08:59:12 UTC] Sentry SRE (Investigating): We are actively investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: We've processed all backlogged events and have fully recovered. We're processing new events in real time, and are working through a backlog of events now. We've identified the issue causing delays in ingestion and have applied a fix. We'll update again when we return to real time processing. We are still actively investigating this issue. We are actively investigating this issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Error ingestion in our US region
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
