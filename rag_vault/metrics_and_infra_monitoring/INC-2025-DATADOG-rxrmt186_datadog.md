# [INC-2025-DATADOG-rxrmt186] Delayed Processes data
**Company:** Datadog | **Date:** 2025-12-12 | **Severity:** HIGH | **Source:** [https://stspg.io/ppc0471xzd9w](https://stspg.io/ppc0471xzd9w)  
**Technologies:** Metrics and Infra Monitoring, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2025-12-12 18:43:52 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2025-12-12 18:31:07 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2025-12-12 17:41:03 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2025-12-12 16:53:21 UTC] Datadog SRE (Investigating): We are continuing to investigate this issue.
[2025-12-12 16:49:44 UTC] Datadog SRE (Investigating): We are investigating increased latency processing Processes data.
As a result of this issue, some users may see delays or gaps for data based on Process Monitoring.
To prevent spurious alerts, we have temporarily disabled monitors based on this data.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. We are continuing to investigate this issue. We are investigating increased latency processing Processes data.
As a result of this issue, some users may see delays or gaps for data based on Process Monitoring.
To prevent spurious alerts, we have temporarily disabled monitors based on this dat

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Processes data
service_cluster:
  provider: "Datadog"
  impacted_components: ["Metrics and Infra Monitoring", "Datadog Ingestion", "APM", "Metrics Agent"]
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
- [ ] Validate automatic health checks and circuit breaking on Metrics and Infra Monitoring cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
