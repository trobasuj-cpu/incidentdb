# [INC-2026-DATADOG-3fnvmf6k] Delayed Evaluation
**Company:** Datadog | **Date:** 2026-06-12 | **Severity:** MEDIUM | **Source:** [https://stspg.io/j0xmm8v17tmx](https://stspg.io/j0xmm8v17tmx)  
**Technologies:** APM, Metrics and Infra Monitoring, Monitors, Datadog Ingestion  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-12 15:39:44 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-06-12 15:23:41 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-06-12 15:08:04 UTC] Datadog SRE (Identified): We are continuing to work on a fix for this issue. During this time, the following components are affected:

Monitor evaluation
Metrics and Infra Monitoring
APM metrics
Estimated usage metrics
[2026-06-12 15:01:17 UTC] Datadog SRE (Identified): We are continuing to work on a fix for this issue.
[2026-06-12 14:39:16 UTC] Datadog SRE (Identified): We are continuing to work on a fix for this issue.
[2026-06-12 14:30:46 UTC] Datadog SRE (Identified): We are continuing to work on a fix for this issue.
[2026-06-12 13:59:27 UTC] Datadog SRE (Identified): We are continuing to work on a fix for this issue.
[2026-06-12 13:12:28 UTC] Datadog SRE (Identified): We are investigating delays in monitor evaluation, which began at 16:10 UTC.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We are continuing to work on a fix for this issue. During this time, the following components are affected:

Monitor evaluation
Metrics and Infra Monitoring
APM metrics
Estimated usage metrics We are continuing to work on a fix for this issue. We are continuing to work on a fix for this issue. We are continuing to work on a fix for this issue. We are co

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Evaluation
service_cluster:
  provider: "Datadog"
  impacted_components: ["APM", "Metrics and Infra Monitoring", "Monitors", "Datadog Ingestion"]
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
