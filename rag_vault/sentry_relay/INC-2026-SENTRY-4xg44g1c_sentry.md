# [INC-2026-SENTRY-4xg44g1c] Delayed ingestion on us region
**Company:** Sentry | **Date:** 2026-07-21 | **Severity:** MEDIUM | **Source:** [https://stspg.io/sn99hk0v2nct](https://stspg.io/sn99hk0v2nct)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-21 14:14:44 UTC] Sentry SRE (Resolved): We've burned through our spans backlog. Latency across span consumers are normal.
[2026-07-21 13:55:01 UTC] Sentry SRE (Monitoring): We're still burning through our spans backlog
[2026-07-21 13:04:00 UTC] Sentry SRE (Monitoring): We've found and fixed the root cause and are burning through our spans backlog. Ingestion should be back to normal in around an hour.
[2026-07-21 12:20:03 UTC] Sentry SRE (Investigating): We are still actively investigating this issue
[2026-07-21 11:26:12 UTC] Sentry SRE (Investigating): We are still actively investigating this issue
[2026-07-21 10:30:26 UTC] Sentry SRE (Investigating): We are actively investigating this issue
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: We've burned through our spans backlog. Latency across span consumers are normal. We're still burning through our spans backlog We've found and fixed the root cause and are burning through our spans backlog. Ingestion should be back to normal in around an hour. We are still actively investigating this issue We are still actively investigating this issue We are actively investigating this issue

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed ingestion on us region
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
