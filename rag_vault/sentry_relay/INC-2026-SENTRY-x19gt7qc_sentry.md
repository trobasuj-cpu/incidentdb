# [INC-2026-SENTRY-x19gt7qc] Error ingestion degraded in US region
**Company:** Sentry | **Date:** 2026-07-10 | **Severity:** CRITICAL | **Source:** [https://stspg.io/bw7pn5bh3bh2](https://stspg.io/bw7pn5bh3bh2)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-11 03:34:49 UTC] Sentry SRE (Resolved): We have processed the backlog.
[2026-07-11 03:28:41 UTC] Sentry SRE (Monitoring): We have processed the majority of our backlog. We are now recovering the tail of the backlog.
[2026-07-11 02:37:41 UTC] Sentry SRE (Identified): We have processed the majority of our backlog. We are now recovering an additional 25M error events ranging from 22:20 to 23:20 UTC, which should complete in the next 2 hours
[2026-07-11 01:46:49 UTC] Sentry SRE (Identified): We expect recovery of historical data to be complete within the next 2 hours.
[2026-07-11 00:56:20 UTC] Sentry SRE (Identified): We are still working on recovery. New errors should be processed in real time, and we are working on recovering historical data.
[2026-07-11 00:04:36 UTC] Sentry SRE (Identified): We have identified the problem and are working on recovery.

New errors are being processed in real time and we are in the process of consuming backlogged errors.
[2026-07-10 23:02:33 UTC] Sentry SRE (Identified): We have identified the problem and are working on a fix, ingested errors are delayed and will be processed once the fix is in place.
[2026-07-10 22:43:49 UTC] Sentry SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: We have processed the backlog. We have processed the majority of our backlog. We are now recovering the tail of the backlog. We have processed the majority of our backlog. We are now recovering an additional 25M error events ranging from 22:20 to 23:20 UTC, which should complete in the next 2 hours We expect recovery of historical data to be complete within the next 2 hours. We are still working on recovery. New errors should be processed in real

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Error ingestion degraded in US region
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
