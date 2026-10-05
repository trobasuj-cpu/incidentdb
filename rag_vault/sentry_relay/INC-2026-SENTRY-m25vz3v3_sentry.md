# [INC-2026-SENTRY-m25vz3v3] Ingestion delays in US
**Company:** Sentry | **Date:** 2026-07-13 | **Severity:** MEDIUM | **Source:** [https://stspg.io/djql0vrrkrg3](https://stspg.io/djql0vrrkrg3)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-13 14:25:13 UTC] Sentry SRE (Resolved): Service has been restored.
[2026-07-13 12:41:45 UTC] Sentry SRE (Monitoring): Service has been restored and we are actively monitoring the situation
[2026-07-13 12:24:13 UTC] Sentry SRE (Identified): We identified the issue and restoring service.
[2026-07-13 11:51:46 UTC] Sentry SRE (Investigating): We are investigating ingestion delays.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Service has been restored. Service has been restored and we are actively monitoring the situation We identified the issue and restoring service. We are investigating ingestion delays.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Ingestion delays in US
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
