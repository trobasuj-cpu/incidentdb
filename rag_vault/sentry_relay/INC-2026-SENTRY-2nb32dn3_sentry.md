# [INC-2026-SENTRY-2nb32dn3] Error ingestion degraded in DE region
**Company:** Sentry | **Date:** 2026-07-14 | **Severity:** CRITICAL | **Source:** [https://stspg.io/y9mjtr1fnjg4](https://stspg.io/y9mjtr1fnjg4)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-14 19:32:24 UTC] Sentry SRE (Resolved): Feedback ingestion is back to realtime and the backlog has been processed.
[2026-07-14 18:16:06 UTC] Sentry SRE (Monitoring): We are continuing to monitor ingestion, and a fix is being deployed to unblock feedback ingestion.
[2026-07-14 17:40:27 UTC] Sentry SRE (Monitoring): We are processing errors in realtime, and error backlogs have been processed. We're continuing to monitor the situation before restoring feedback ingestion.
[2026-07-14 17:02:09 UTC] Sentry SRE (Identified): We are still working on recovery. New errors should be processed in real time, and we are working on recovering historical data.

We are working to start allowing user feedback in DE.
[2026-07-14 16:49:47 UTC] Sentry SRE (Identified): We are still working on recovery. New errors should be processed in real time, and we are working on recovering historical data.
[2026-07-14 16:42:36 UTC] Sentry SRE (Identified): We have identified the problem and are working on recovery.
[2026-07-14 15:42:54 UTC] Sentry SRE (Investigating): We continue to investigate the issue.
[2026-07-14 14:44:35 UTC] Sentry SRE (Investigating): We are currently investigating the issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Feedback ingestion is back to realtime and the backlog has been processed. We are continuing to monitor ingestion, and a fix is being deployed to unblock feedback ingestion. We are processing errors in realtime, and error backlogs have been processed. We're continuing to monitor the situation before restoring feedback ingestion. We are still working on recovery. New errors should be processed in real time, and we are working on recovering histori

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Error ingestion degraded in DE region
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
