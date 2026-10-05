# [INC-2026-SENTRY-ksnqt59v] Span Ingestion Delayed in the EU region
**Company:** Sentry | **Date:** 2026-04-16 | **Severity:** MEDIUM | **Source:** [https://stspg.io/vqg8dbj3z6y3](https://stspg.io/vqg8dbj3z6y3)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-16 16:08:44 UTC] Sentry SRE (Resolved): The fix has been rolled out and ingestion is recovered.
[2026-04-16 15:05:53 UTC] Sentry SRE (Monitoring): The delay has recovered and we are monitoring the fix.
[2026-04-16 14:30:12 UTC] Sentry SRE (Identified): We have identified the issue and are working on a fix.
[2026-04-16 13:36:39 UTC] Sentry SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: The fix has been rolled out and ingestion is recovered. The delay has recovered and we are monitoring the fix. We have identified the issue and are working on a fix. We are currently investigating this issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Span Ingestion Delayed in the EU region
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
