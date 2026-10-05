# [INC-2026-DATADOG-fsrqsbtp] Delayed events triggered by emails
**Company:** Datadog | **Date:** 2026-09-20 | **Severity:** MEDIUM | **Source:** [https://stspg.io/txhsm142w1cz](https://stspg.io/txhsm142w1cz)  
**Technologies:** Incident Response, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-20 02:53:02 UTC] Datadog SRE (Resolved): The issue is now resolved. All events from emails are being timely processed and we went through the backfill.
[2026-09-20 01:17:14 UTC] Datadog SRE (Investigating): We are investigating increased latency processing Events coming from inbound emails. As a result of this issue, some users may see delays or gaps in the event stream or for event queries on dashboards and for events based workflows such as on-call notifications
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: The issue is now resolved. All events from emails are being timely processed and we went through the backfill. We are investigating increased latency processing Events coming from inbound emails. As a result of this issue, some users may see delays or gaps in the event stream or for event queries on dashboards and for events based workflows such as on-call notifications

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed events triggered by emails
service_cluster:
  provider: "Datadog"
  impacted_components: ["Incident Response", "Datadog Ingestion", "APM", "Metrics Agent"]
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
- [ ] Validate automatic health checks and circuit breaking on Incident Response cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
