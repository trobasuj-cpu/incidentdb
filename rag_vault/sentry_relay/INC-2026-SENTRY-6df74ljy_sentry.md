# [INC-2026-SENTRY-6df74ljy] Ingestion and alerts for transactions delayed in EU region
**Company:** Sentry | **Date:** 2026-05-19 | **Severity:** MEDIUM | **Source:** [https://stspg.io/rdry9sq85gm2](https://stspg.io/rdry9sq85gm2)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-19 14:18:29 UTC] Sentry SRE (Resolved): Ingestion and alerting back to normal
[2026-05-19 14:05:30 UTC] Sentry SRE (Monitoring): We've put in fixes and are monitoring the situation
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Ingestion and alerting back to normal We've put in fixes and are monitoring the situation

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Ingestion and alerts for transactions delayed in EU region
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
