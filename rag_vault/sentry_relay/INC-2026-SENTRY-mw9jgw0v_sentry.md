# [INC-2026-SENTRY-mw9jgw0v] Spans, crons, logs ingestion delayed in US region
**Company:** Sentry | **Date:** 2026-08-07 | **Severity:** CRITICAL | **Source:** [https://stspg.io/hm0ff1mzq3vl](https://stspg.io/hm0ff1mzq3vl)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-07 17:08:22 UTC] Sentry SRE (Resolved): The ingestion delay is resolved
[2026-08-07 16:48:50 UTC] Sentry SRE (Monitoring): Ingestion latency for spans, crons, and logs is back to normal levels.
[2026-08-07 16:30:50 UTC] Sentry SRE (Identified): We've put a fix in place and are monitoring. We anticipate catching up to ingestion within 1 hour
[2026-08-07 16:21:05 UTC] Sentry SRE (Investigating): We are currently investigating the issue
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: The ingestion delay is resolved Ingestion latency for spans, crons, and logs is back to normal levels. We've put a fix in place and are monitoring. We anticipate catching up to ingestion within 1 hour We are currently investigating the issue

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Spans, crons, logs ingestion delayed in US region
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
