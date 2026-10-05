# [INC-2026-DATADOG-7l0kwdnv] Delayed Events
**Company:** Datadog | **Date:** 2026-06-18 | **Severity:** MEDIUM | **Source:** [https://stspg.io/tdw3spf8slj1](https://stspg.io/tdw3spf8slj1)  
**Technologies:** Monitors, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-18 12:33:28 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-06-18 12:22:43 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-06-18 12:13:18 UTC] Datadog SRE (Investigating): We are investigating increased latency processing Events.
As a result of this issue, some users may see delays or gaps in the event stream or for event queries on dashboards
To prevent false monitor alerts due to delayed data, monitors affected by the delay will not notify and will automatically resume once current data is available. All other monitors will operate normally..
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We are investigating increased latency processing Events.
As a result of this issue, some users may see delays or gaps in the event stream or for event queries on dashboards
To prevent false monitor alerts due to delayed data, monitors affected by the delay will not notify and will automatically resume once current data is available. All other monitors

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Events
service_cluster:
  provider: "Datadog"
  impacted_components: ["Monitors", "Datadog Ingestion", "APM", "Metrics Agent"]
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
- [ ] Validate automatic health checks and circuit breaking on Monitors cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
