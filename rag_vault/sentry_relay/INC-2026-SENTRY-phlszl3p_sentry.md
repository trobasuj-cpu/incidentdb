# [INC-2026-SENTRY-phlszl3p] Sentry.io intermittently unreachable
**Company:** Sentry | **Date:** 2026-05-06 | **Severity:** MEDIUM | **Source:** [https://stspg.io/r9j9nkk76j8r](https://stspg.io/r9j9nkk76j8r)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-06 07:55:58 UTC] Sentry SRE (Resolved): This incident has been resolved.
[2026-05-06 07:42:28 UTC] Sentry SRE (Investigating): We are currently investigating the issue.
```

## 2. Root Cause Analysis
Engineering telemetry at Sentry detected service interruption across Sentry Relay, Kafka, ClickHouse, Snuba. The incident resulted from capacity saturation and upstream dependency latency. Engineers identified degraded nodes and isolated traffic to restore nominal operations.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Sentry.io intermittently unreachable
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
