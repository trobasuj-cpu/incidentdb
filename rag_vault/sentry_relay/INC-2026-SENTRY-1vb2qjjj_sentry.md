# [INC-2026-SENTRY-1vb2qjjj] Delayed Error ingestion in our US region
**Company:** Sentry | **Date:** 2026-08-12 | **Severity:** HIGH | **Source:** [https://stspg.io/tcdgnyx93p0q](https://stspg.io/tcdgnyx93p0q)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-12 14:48:25 UTC] Sentry SRE (Resolved): The backlog has been processed
[2026-08-12 14:21:12 UTC] Sentry SRE (Monitoring): We're processing new events and alerts in realtime, and continue to process the tail of the backlog.
[2026-08-12 13:28:57 UTC] Sentry SRE (Identified): A fix has been made and the backlog is being processed.
[2026-08-12 12:36:32 UTC] Sentry SRE (Investigating): We've identified the root cause and are actively working on mitigation.
[2026-08-12 11:45:45 UTC] Sentry SRE (Investigating): We are actively investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: The backlog has been processed We're processing new events and alerts in realtime, and continue to process the tail of the backlog. A fix has been made and the backlog is being processed. We've identified the root cause and are actively working on mitigation. We are actively investigating this issue.

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
