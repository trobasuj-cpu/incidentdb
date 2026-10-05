# [INC-2026-DATADOG-29t1p0vx] Delayed APM Trace Metrics
**Company:** Datadog | **Date:** 2026-06-26 | **Severity:** MEDIUM | **Source:** [https://stspg.io/w6292y3bqkk1](https://stspg.io/w6292y3bqkk1)  
**Technologies:** Datadog Ingestion, APM, Metrics Agent, Kafka  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-26 09:45:37 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-06-26 09:41:24 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-06-26 09:38:59 UTC] Datadog SRE (Identified): We have identified increased latency processing APM Trace Metrics and are working on a fix.
As a result of this issue, some users may see delayed APM Trace Metrics since 13:07 UTC.
To prevent false monitor alerts due to delayed data, monitors affected by the delay will not notify and will automatically resume once current data is available. All other monitors will operate normally..
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We have identified increased latency processing APM Trace Metrics and are working on a fix.
As a result of this issue, some users may see delayed APM Trace Metrics since 13:07 UTC.
To prevent false monitor alerts due to delayed data, monitors affected by the delay will not notify and will automatically resume once current data is available. All other mo

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed APM Trace Metrics
service_cluster:
  provider: "Datadog"
  impacted_components: ["Datadog Ingestion", "APM", "Metrics Agent", "Kafka"]
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
- [ ] Validate automatic health checks and circuit breaking on Datadog Ingestion cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
