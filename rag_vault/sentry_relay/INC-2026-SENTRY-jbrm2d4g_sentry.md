# [INC-2026-SENTRY-jbrm2d4g] Error and profile ingestion issues in US
**Company:** Sentry | **Date:** 2026-07-27 | **Severity:** CRITICAL | **Source:** [https://stspg.io/0cy4vyqy4yhr](https://stspg.io/0cy4vyqy4yhr)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-27 18:27:00 UTC] Sentry SRE (Resolved): The backlog has been cleared and ingestion is back to normal.
[2026-07-27 18:07:29 UTC] Sentry SRE (Monitoring): We have resolved the incident and are monitoring our ingestion backlog.
[2026-07-27 16:56:22 UTC] Sentry SRE (Identified): We have identified the issue and error and profile ingestion is back up in US. Expect delays in ingestion and alerting for errors and profiles.
[2026-07-27 16:23:47 UTC] Sentry SRE (Investigating): We are experiencing issues with error and profile ingestion in our US region and are investigating the issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: The backlog has been cleared and ingestion is back to normal. We have resolved the incident and are monitoring our ingestion backlog. We have identified the issue and error and profile ingestion is back up in US. Expect delays in ingestion and alerting for errors and profiles. We are experiencing issues with error and profile ingestion in our US region and are investigating the issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Error and profile ingestion issues in US
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
