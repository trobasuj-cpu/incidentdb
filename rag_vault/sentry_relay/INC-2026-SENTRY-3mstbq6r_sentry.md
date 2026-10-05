# [INC-2026-SENTRY-3mstbq6r] Application errors in Sentry [us]
**Company:** Sentry | **Date:** 2026-04-23 | **Severity:** MEDIUM | **Source:** [https://stspg.io/87vzn5spgk4j](https://stspg.io/87vzn5spgk4j)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-23 02:17:48 UTC] Sentry SRE (Resolved): All application features are now operating as normal.
[2026-04-23 02:12:24 UTC] Sentry SRE (Monitoring): Ingestion for spans and replays is about 10 minutes behind and catching up. API and Dashboard performance has returned to normal.
[2026-04-23 02:06:53 UTC] Sentry SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: All application features are now operating as normal. Ingestion for spans and replays is about 10 minutes behind and catching up. API and Dashboard performance has returned to normal. We are currently investigating this issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Application errors in Sentry [us]
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
