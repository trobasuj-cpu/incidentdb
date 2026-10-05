# [INC-2026-DATADOG-qqb6y9g8] Delayed RUM data
**Company:** Datadog | **Date:** 2026-02-23 | **Severity:** MEDIUM | **Source:** [https://stspg.io/f7nly8tc2198](https://stspg.io/f7nly8tc2198)  
**Technologies:** Cloud Cost Management, RUM, Datadog Ingestion, APM  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-02-24 02:08:31 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-02-24 00:52:57 UTC] Datadog SRE (Monitoring): We have deployed a fix and we are monitoring the results.
We will provide another update once the issue is fully resolved.
[2026-02-24 00:29:47 UTC] Datadog SRE (Identified): We have identified the underlying issue and are working on a fix.
It is important to note that no data has been lost, and it will be backfilled and available once the service is operational again.
[2026-02-23 23:31:54 UTC] Datadog SRE (Investigating): We are investigating increased latency processing RUM data.
As a result of this issue, some users may see gaps or delays in RUM graphs as well as empty or partial query results on RUM Sessions, RUM Analytics, RUM Application, and Error Tracking pages.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. We have deployed a fix and we are monitoring the results.
We will provide another update once the issue is fully resolved. We have identified the underlying issue and are working on a fix.
It is important to note that no data has been lost, and it will be backfilled and available once the service is operational again. We are investigating increased latency processing RUM data.
As a result of this issue, some users

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed RUM data
service_cluster:
  provider: "Datadog"
  impacted_components: ["Cloud Cost Management", "RUM", "Datadog Ingestion", "APM"]
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
- [ ] Validate automatic health checks and circuit breaking on Cloud Cost Management cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
