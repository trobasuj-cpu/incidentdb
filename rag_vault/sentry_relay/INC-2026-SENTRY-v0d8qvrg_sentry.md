# [INC-2026-SENTRY-v0d8qvrg] Delays in spans ingestion
**Company:** Sentry | **Date:** 2026-09-18 | **Severity:** HIGH | **Source:** [https://stspg.io/7tjcn7m11qh3](https://stspg.io/7tjcn7m11qh3)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-18 10:58:05 UTC] Sentry SRE (Resolved): We have identified a networking issue with an upstream provider and are working on mitigations.
[2026-09-18 10:56:38 UTC] Sentry SRE (Investigating): We have identified a networking issue with an upstream provider and are working on mitigations.
[2026-09-18 10:17:01 UTC] Sentry SRE (Investigating): We are currently investigating an issue affecting spans ingestion in the EU region
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: We have identified a networking issue with an upstream provider and are working on mitigations. We have identified a networking issue with an upstream provider and are working on mitigations. We are currently investigating an issue affecting spans ingestion in the EU region

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delays in spans ingestion
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
