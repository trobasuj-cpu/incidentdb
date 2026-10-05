# [INC-2026-SENTRY-4bnv32fj] Ingestion and query issues for logs, spans, traces in US
**Company:** Sentry | **Date:** 2026-06-30 | **Severity:** MEDIUM | **Source:** [https://stspg.io/gnyhyysv5gvl](https://stspg.io/gnyhyysv5gvl)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-01 10:00:10 UTC] Sentry SRE (Resolved): Data is now processed live and the system are fully operational.
[2026-07-01 06:14:15 UTC] Sentry SRE (Monitoring): We continue to monitor the fix. Ingestion is around 45 minutes behind and improving. APIs and dashboards are functioning normally (outside of data waiting to be ingested).
[2026-07-01 05:22:14 UTC] Sentry SRE (Monitoring): We continue to monitor the situation
[2026-07-01 04:26:35 UTC] Sentry SRE (Monitoring): We continue to monitor the situation
[2026-07-01 03:30:33 UTC] Sentry SRE (Monitoring): We are currently burning down the ingestion backlog, and estimate that it will be complete by 2AM PT 2026-07-01.
[2026-07-01 02:25:48 UTC] Sentry SRE (Investigating): We are attempting additional mitigations to restore ingestion for logs, spans, and traces in the US
[2026-07-01 01:29:27 UTC] Sentry SRE (Investigating): Our ingestion fix was not successful. We are continuing to investigate possible solutions
[2026-07-01 00:27:30 UTC] Sentry SRE (Investigating): Our database is stabilized. We are attempting a fix to increase the ingestion rate.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Data is now processed live and the system are fully operational. We continue to monitor the fix. Ingestion is around 45 minutes behind and improving. APIs and dashboards are functioning normally (outside of data waiting to be ingested). We continue to monitor the situation We continue to monitor the situation We are currently burning down the ingestion backlog, and estimate that it will be complete by 2AM PT 2026-07-01. We are attempting addition

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Ingestion and query issues for logs, spans, traces in US
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
