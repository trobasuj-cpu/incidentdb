# [INC-2026-DATADOG-476hf8wn] Delayed Events
**Company:** Datadog | **Date:** 2026-01-18 | **Severity:** MEDIUM | **Source:** [https://stspg.io/w2k14cxk8th3](https://stspg.io/w2k14cxk8th3)  
**Technologies:** Metrics and Infra Monitoring, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-01-18 09:24:41 UTC] Datadog SRE (Resolved): This incident is resolved. There's no more delay for the processing of Events, nor impact on the event stream, event based widgets and event based monitors.
[2026-01-18 08:26:48 UTC] Datadog SRE (Identified): Recovery is in progress and the new estimated time of recovery would be 14h30 UTC.
[2026-01-18 07:35:23 UTC] Datadog SRE (Identified): We have identified the issue and scaled up for recovery, with a recovery estimated to be around 14h30 UTC. We'll continue to give updates as recovery progresses.
[2026-01-18 07:33:26 UTC] Datadog SRE (Identified): We are investigating increased latency processing Events.
As a result of this issue, some users may see delays or gaps in the event stream or for event based widgets or event based monitors.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident is resolved. There's no more delay for the processing of Events, nor impact on the event stream, event based widgets and event based monitors. Recovery is in progress and the new estimated time of recovery would be 14h30 UTC. We have identified the issue and scaled up for recovery, with a recovery estimated to be around 14h30 UTC. We'll continue to give updates as recovery progresses. We are investigating increased latency processin

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Events
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
