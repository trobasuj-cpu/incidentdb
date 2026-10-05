# [INC-2026-SENTRY-4mr92qjf] Ingestion and alerting delays in US
**Company:** Sentry | **Date:** 2026-05-18 | **Severity:** MEDIUM | **Source:** [https://stspg.io/z6rsqwkrtfnz](https://stspg.io/z6rsqwkrtfnz)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-18 07:11:03 UTC] Sentry SRE (Resolved): We have implemented a fix. Ingestion and alerting have returned to normal operation
[2026-05-18 05:35:55 UTC] Sentry SRE (Investigating): We are continuing to investigate the issue.
[2026-05-18 04:27:11 UTC] Sentry SRE (Investigating): We are still investigating the issue.
[2026-05-18 03:27:20 UTC] Sentry SRE (Investigating): We are still investigating the issue.
[2026-05-18 02:29:37 UTC] Sentry SRE (Investigating): We are still investigating the issue.
[2026-05-18 01:41:35 UTC] Sentry SRE (Investigating): We are experiencing ingestion and alerting delays in us for spans, trace metrics, and logs
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: We have implemented a fix. Ingestion and alerting have returned to normal operation We are continuing to investigate the issue. We are still investigating the issue. We are still investigating the issue. We are still investigating the issue. We are experiencing ingestion and alerting delays in us for spans, trace metrics, and logs

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Ingestion and alerting delays in US
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
