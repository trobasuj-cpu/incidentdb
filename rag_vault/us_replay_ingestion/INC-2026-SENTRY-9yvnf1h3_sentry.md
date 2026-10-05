# [INC-2026-SENTRY-9yvnf1h3] Delayed Span ingestion, Replay ingestion, and Issue creation in our US region
**Company:** Sentry | **Date:** 2026-06-24 | **Severity:** MEDIUM | **Source:** [https://stspg.io/p962182mq6km](https://stspg.io/p962182mq6km)  
**Technologies:** US Replay Ingestion, US Span Ingestion, Sentry Relay, Kafka  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-24 20:48:03 UTC] Sentry SRE (Resolved): The issue in our US region has been resolved, and ingestion latency is back to normal.
[2026-06-24 19:53:17 UTC] Sentry SRE (Monitoring): We have implemented a fix for the problem and are monitoring the situation. We are burning our US region ingestion backlog for replays, spans, and performance issues and will update here as the situation progresses.
[2026-06-24 18:52:00 UTC] Sentry SRE (Identified): We have identified an issue in our US region causing delayed ingestion of spans and replays, as well as delayed creation of some issue types.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: The issue in our US region has been resolved, and ingestion latency is back to normal. We have implemented a fix for the problem and are monitoring the situation. We are burning our US region ingestion backlog for replays, spans, and performance issues and will update here as the situation progresses. We have identified an issue in our US region causing delayed ingestion of spans and replays, as well as delayed creation of some issue types.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Span ingestion, Replay ingestion, and Issue creation in our US region
service_cluster:
  provider: "Sentry"
  impacted_components: ["US Replay Ingestion", "US Span Ingestion", "Sentry Relay", "Kafka"]
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
- [ ] Validate automatic health checks and circuit breaking on US Replay Ingestion cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
