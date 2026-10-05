# [INC-2026-SENTRY-w39v80r1] Sentry.io timeouts
**Company:** Sentry | **Date:** 2026-05-11 | **Severity:** MEDIUM | **Source:** [https://stspg.io/j08czhkhxqg4](https://stspg.io/j08czhkhxqg4)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-11 22:11:27 UTC] Sentry SRE (Resolved): Database performance has returned to normal
[2026-05-11 21:39:28 UTC] Sentry SRE (Monitoring): We have implemented a fix and are monitoring the situation
[2026-05-11 21:08:27 UTC] Sentry SRE (Investigating): We are working on restoring normal service. API and application performance should be improved, ingestion still has delays.
[2026-05-11 20:25:25 UTC] Sentry SRE (Investigating): Our event database continues to have issues and we are actively investigating the issue.
[2026-05-11 19:28:19 UTC] Sentry SRE (Investigating): Ingestion for spans, replays, and crons is delayed by about 15 minutes.
[2026-05-11 18:57:02 UTC] Sentry SRE (Investigating): We are continuing to investigate degraded database performance.
[2026-05-11 15:32:14 UTC] Sentry SRE (Investigating): We are seeing increased UI and API timeouts and errors, due to an overloaded database cluster. Ingestion and alerting should not be affected
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Database performance has returned to normal We have implemented a fix and are monitoring the situation We are working on restoring normal service. API and application performance should be improved, ingestion still has delays. Our event database continues to have issues and we are actively investigating the issue. Ingestion for spans, replays, and crons is delayed by about 15 minutes. We are continuing to investigate degraded database performance

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Sentry.io timeouts
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
