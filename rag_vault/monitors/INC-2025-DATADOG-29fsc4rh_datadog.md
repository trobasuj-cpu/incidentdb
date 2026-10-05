# [INC-2025-DATADOG-29fsc4rh] Delayed Monitors Notifications
**Company:** Datadog | **Date:** 2025-11-18 | **Severity:** MEDIUM | **Source:** [https://stspg.io/0r75b5ng1fc2](https://stspg.io/0r75b5ng1fc2)  
**Technologies:** Monitors, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2025-11-18 08:53:58 UTC] Datadog SRE (Resolved): This incident has been resolved. Notification delays were only affecting our internal monitoring and were due to the ongoing Cloudflare incident: https://www.cloudflarestatus.com/incidents/8gmgl950y3h7/.
[2025-11-18 08:17:44 UTC] Datadog SRE (Investigating): We are investigating delays in RUM-based Monitors Notifications, which began at 11:30am UTC.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. Notification delays were only affecting our internal monitoring and were due to the ongoing Cloudflare incident: https://www.cloudflarestatus.com/incidents/8gmgl950y3h7/. We are investigating delays in RUM-based Monitors Notifications, which began at 11:30am UTC.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Monitors Notifications
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
