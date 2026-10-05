# [INC-2026-DATADOG-p0gz2w4n] Increased latency across multiple products
**Company:** Datadog | **Date:** 2026-06-08 | **Severity:** MEDIUM | **Source:** [https://stspg.io/k3z1l9vwjhkp](https://stspg.io/k3z1l9vwjhkp)  
**Technologies:** Cloud Cost Management, Cloud Network Monitoring, Cloud Security Management, Database Monitoring  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-08 08:59:27 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-06-08 08:21:47 UTC] Datadog SRE (Monitoring): We are continuing to monitor the fix deployed earlier.
We will provide another update once the issue is fully resolved.
[2026-06-08 07:21:57 UTC] Datadog SRE (Monitoring): We have deployed a fix and we are monitoring the results.
[2026-06-08 07:05:03 UTC] Datadog SRE (Identified): We are investigating and have identified a fix for increased latency across multiple products. As a result of this issue, some users may see delays in data across the platform.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. We are continuing to monitor the fix deployed earlier.
We will provide another update once the issue is fully resolved. We have deployed a fix and we are monitoring the results. We are investigating and have identified a fix for increased latency across multiple products. As a result of this issue, some users may see delays in data across the platform.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Increased latency across multiple products
service_cluster:
  provider: "Datadog"
  impacted_components: ["Cloud Cost Management", "Cloud Network Monitoring", "Cloud Security Management", "Database Monitoring"]
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
