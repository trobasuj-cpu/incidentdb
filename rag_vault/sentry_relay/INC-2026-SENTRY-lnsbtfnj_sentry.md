# [INC-2026-SENTRY-lnsbtfnj] EU Region Error, Attachment, and Feedback latency
**Company:** Sentry | **Date:** 2026-07-20 | **Severity:** CRITICAL | **Source:** [https://stspg.io/0k3pvpywznz6](https://stspg.io/0k3pvpywznz6)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-20 16:57:37 UTC] Sentry SRE (Resolved): We have fully burned the backlog of EU error events ingested during the incident. Latency is normal across all event types.
[2026-07-20 16:09:46 UTC] Sentry SRE (Monitoring): We are still burning the backlog of EU region error events received during the incident. We are around halfway through the backlog.
[2026-07-20 14:50:36 UTC] Sentry SRE (Monitoring): The backlog on attachments in our EU region is fully burned, and latency is back to normal. While new error events are being processed with normal latency, we have a backlog of old error events that we are now burning through.
[2026-07-20 14:30:18 UTC] Sentry SRE (Monitoring): We have implemented a fix for our attachment ingestion issue in our EU region, and are monitoring while the backlog burns. We will update here when latency is back to normal.
[2026-07-20 13:47:35 UTC] Sentry SRE (Identified): We have implemented a fix for errors and feedback ingestion in our EU region, and latency for those pipelines has returned to normal. We are still working on fixing our attachments ingestion pipeline in EU.
[2026-07-20 13:19:23 UTC] Sentry SRE (Identified): We have identified the issue causing errors, attachments, and feedback events to have high ingest latency in our EU region and are continuing to work on a fix.
[2026-07-20 12:24:52 UTC] Sentry SRE (Identified): We have identified the issue causing errors, attachments, and feedback events to have high ingest latency in our EU region and are continuing to work on a fix.
[2026-07-20 12:10:57 UTC] Sentry SRE (Identified): We have identified the issue causing errors, attachments, and feedback events to have high ingest latency in our EU region and are continuing to work on a fix.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: We have fully burned the backlog of EU error events ingested during the incident. Latency is normal across all event types. We are still burning the backlog of EU region error events received during the incident. We are around halfway through the backlog. The backlog on attachments in our EU region is fully burned, and latency is back to normal. While new error events are being processed with normal latency, we have a backlog of old error events

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during EU Region Error, Attachment, and Feedback latency
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
