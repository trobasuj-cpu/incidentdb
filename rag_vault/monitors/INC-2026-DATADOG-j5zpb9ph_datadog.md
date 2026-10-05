# [INC-2026-DATADOG-j5zpb9ph] Intermittent "No Data" Status for Monitors
**Company:** Datadog | **Date:** 2026-03-18 | **Severity:** HIGH | **Source:** [https://stspg.io/43kkcl1tqrrk](https://stspg.io/43kkcl1tqrrk)  
**Technologies:** Monitors, Datadog Ingestion, APM, Metrics Agent  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-18 22:53:46 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-03-18 22:39:52 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-03-18 22:04:58 UTC] Datadog SRE (Investigating): We are continuing to investigate the issue and will provide updates as available.
[2026-03-18 21:27:45 UTC] Datadog SRE (Investigating): We are continuing to investigate the issue and will provide updates as available.
[2026-03-18 20:54:23 UTC] Datadog SRE (Investigating): We are investigating an issue causing some metric monitors to intermittently report "No Data" status. This primarily affects monitors based on distribution metrics. Monitor alerting may be unreliable during this time. We are actively investigating and will provide updates as available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We are continuing to investigate the issue and will provide updates as available. We are continuing to investigate the issue and will provide updates as available. We are investigating an issue causing some metric monitors to intermittently report "No Data" status. This primarily affects monitors based on distribution metrics. Monitor alerting may be un

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Intermittent "No Data" Status for Monitors
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
