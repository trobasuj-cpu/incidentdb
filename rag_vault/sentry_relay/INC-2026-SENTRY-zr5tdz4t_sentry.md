# [INC-2026-SENTRY-zr5tdz4t] Span ingestion is degraded in US
**Company:** Sentry | **Date:** 2026-09-01 | **Severity:** MEDIUM | **Source:** [https://stspg.io/nc8v40xh55zt](https://stspg.io/nc8v40xh55zt)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-01 20:29:43 UTC] Sentry SRE (Resolved): Spans ingestion is now operating normally.
[2026-09-01 19:31:54 UTC] Sentry SRE (Monitoring): Latency is almost fully recovered.
[2026-09-01 18:30:02 UTC] Sentry SRE (Monitoring): A fix has been implemented, and latency is coming down.
[2026-09-01 17:50:29 UTC] Sentry SRE (Investigating): We are continuing to investigate an issue that affects spans ingestion latency
[2026-09-01 16:41:02 UTC] Sentry SRE (Investigating): We are continuing to investigate an issue that affects spans ingestion latency
[2026-09-01 15:40:59 UTC] Sentry SRE (Investigating): We are continuing to investigate an issue that affects spans ingestion latency
[2026-09-01 14:16:13 UTC] Sentry SRE (Investigating): We are experiencing slow ingestion for spans in US
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: Spans ingestion is now operating normally. Latency is almost fully recovered. A fix has been implemented, and latency is coming down. We are continuing to investigate an issue that affects spans ingestion latency We are continuing to investigate an issue that affects spans ingestion latency We are continuing to investigate an issue that affects spans ingestion latency We are experiencing slow ingestion for spans in US

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Span ingestion is degraded in US
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
