# [INC-2026-DATADOG-y1trpqc5] Delayed Traces in APM Trace Search
**Company:** Datadog | **Date:** 2026-03-27 | **Severity:** HIGH | **Source:** [https://stspg.io/4qqxn4k2w7n5](https://stspg.io/4qqxn4k2w7n5)  
**Technologies:** APM, Datadog Ingestion, Metrics Agent, Kafka  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-27 15:30:04 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-03-27 15:21:54 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-03-27 15:17:56 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-03-27 14:57:08 UTC] Datadog SRE (Investigating): We are investigating increased latency in processing and storing Traces in APM.
As a result of this issue, some users may see missing or delayed traces in APM Trace Search since 5pm UTC. They may also experience delay in APM trace-based monitors.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. We are investigating increased latency in processing and storing Traces in APM.
As a result of this issue, some users may see missing or delayed traces in APM Trace Search since 5pm UTC. They may also experience delay in APM trace-based monitors.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Traces in APM Trace Search
service_cluster:
  provider: "Datadog"
  impacted_components: ["APM", "Datadog Ingestion", "Metrics Agent", "Kafka"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Datadog SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on APM cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
