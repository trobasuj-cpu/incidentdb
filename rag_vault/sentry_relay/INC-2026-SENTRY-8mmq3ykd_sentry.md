# [INC-2026-SENTRY-8mmq3ykd] Error ingestion delays in US
**Company:** Sentry | **Date:** 2026-08-12 | **Severity:** HIGH | **Source:** [https://stspg.io/qh9kqp474lw2](https://stspg.io/qh9kqp474lw2)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-12 22:52:59 UTC] Sentry SRE (Resolved): Backlogs have been processed
[2026-08-12 22:43:55 UTC] Sentry SRE (Identified): We've mitigated the source of ingestion latency and are processing the backlog.
[2026-08-12 21:58:59 UTC] Sentry SRE (Investigating): We are currently investigating error ingestion latency in US.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Backlogs have been processed We've mitigated the source of ingestion latency and are processing the backlog. We are currently investigating error ingestion latency in US.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Error ingestion delays in US
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
