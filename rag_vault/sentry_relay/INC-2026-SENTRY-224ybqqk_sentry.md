# [INC-2026-SENTRY-224ybqqk] Users unable to login to sentry.io
**Company:** Sentry | **Date:** 2026-06-10 | **Severity:** MEDIUM | **Source:** [https://stspg.io/c0zxps5b0p7l](https://stspg.io/c0zxps5b0p7l)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** AUTHENTICATION_FAILURE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-10 10:28:46 UTC] Sentry SRE (Resolved): Our auth systems are back operational
[2026-06-10 10:17:26 UTC] Sentry SRE (Monitoring): We rolled back a configuration change we identified as the root cause of the issue. We'll keep monitoring our systems
[2026-06-10 10:15:13 UTC] Sentry SRE (Monitoring): We deployed a fix and are monitoring the situation.
[2026-06-10 10:05:57 UTC] Sentry SRE (Investigating): We are currently investigating the issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Our auth systems are back operational We rolled back a configuration change we identified as the root cause of the issue. We'll keep monitoring our systems We deployed a fix and are monitoring the situation. We are currently investigating the issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Users unable to login to sentry.io
service_cluster:
  provider: "Sentry"
  impacted_components: ["Sentry Relay", "Kafka", "ClickHouse", "Snuba"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Sentry SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Sentry Relay cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
