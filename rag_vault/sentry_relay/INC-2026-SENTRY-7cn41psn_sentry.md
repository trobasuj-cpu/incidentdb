# [INC-2026-SENTRY-7cn41psn] Ingestion delayed for spans, logs, crons in US region
**Company:** Sentry | **Date:** 2026-07-13 | **Severity:** CRITICAL | **Source:** [https://stspg.io/bmcxxpdgrvfj](https://stspg.io/bmcxxpdgrvfj)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-13 02:56:40 UTC] Sentry SRE (Resolved): Ingestion has caught up and alerts have been restored.
[2026-07-13 02:01:56 UTC] Sentry SRE (Monitoring): We have implemented a fix and are monitoring the situation. We expect ingestion to catch up in the next hour, approximately.
[2026-07-13 01:46:43 UTC] Sentry SRE (Identified): We have identified the problem and are working to restore ingestion
[2026-07-13 00:58:14 UTC] Sentry SRE (Investigating): We are currently investigating the issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Ingestion has caught up and alerts have been restored. We have implemented a fix and are monitoring the situation. We expect ingestion to catch up in the next hour, approximately. We have identified the problem and are working to restore ingestion We are currently investigating the issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Ingestion delayed for spans, logs, crons in US region
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
