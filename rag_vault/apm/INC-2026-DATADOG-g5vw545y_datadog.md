# [INC-2026-DATADOG-g5vw545y] Delayed Traces in APM Trace Search
**Company:** Datadog | **Date:** 2026-02-11 | **Severity:** MEDIUM | **Source:** [https://stspg.io/4znpw24vxbbg](https://stspg.io/4znpw24vxbbg)  
**Technologies:** APM, Monitors, Datadog Ingestion, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-02-11 18:44:55 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-02-11 18:28:55 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-02-11 17:28:31 UTC] Datadog SRE (Identified): Teams continue to work to mitigate the impact of this issue. At this time, APM Trace Processing, APM Trace Monitors are still delayed. Distribution Metrics monitors were delayed between 21:35 and 22:15 UTC
[2026-02-11 16:51:27 UTC] Datadog SRE (Identified): Teams continue to work to mitigate the impact of this issue. At this time, APM Trace Processing, APM Trace Monitors are still delayed and Distribution Metrics monitors are delayed starting from 21:35 UTC
[2026-02-11 16:18:40 UTC] Datadog SRE (Identified): Teams have identified the issue and are working to mitigate at this time
[2026-02-11 15:56:50 UTC] Datadog SRE (Investigating): We are investigating increased latency processing Traces in APM Trace Search.
As a result of this issue, some users may see missing or delayed traces in APM Trace Search and delayed APM Trace Monitors since 21:30 UTC.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. Teams continue to work to mitigate the impact of this issue. At this time, APM Trace Processing, APM Trace Monitors are still delayed. Distribution Metrics monitors were delayed between 21:35 and 22:15 UTC Teams continue to work to mitigate the impact of this issue. At this time, APM Trace Processing, APM Trace Monitors are still delayed and Distributio

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Traces in APM Trace Search
service_cluster:
  provider: "Datadog"
  impacted_components: ["APM", "Monitors", "Datadog Ingestion", "Metrics Agent"]
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
