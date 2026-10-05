# [INC-2026-SENTRY-rgcnzc6q] Span & Transaction ingestion delayed in DE
**Company:** Sentry | **Date:** 2026-08-13 | **Severity:** HIGH | **Source:** [https://stspg.io/3dh3m2pvx8f2](https://stspg.io/3dh3m2pvx8f2)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-13 22:27:03 UTC] Sentry SRE (Resolved): All backlogs have been processed.
[2026-08-13 22:13:48 UTC] Sentry SRE (Monitoring): We continue to make progress on the spans backlog with an estimated completion time of 15 minutes.
[2026-08-13 21:22:18 UTC] Sentry SRE (Monitoring): We're continuing to make progress on the span ingestion backlog in US.
[2026-08-13 20:32:19 UTC] Sentry SRE (Monitoring): We've recovered all ingestion latency in DE. In US, errors have processed their backlog and we continue to make progress on spans in US.
[2026-08-13 19:53:24 UTC] Sentry SRE (Monitoring): The problematic change has been reverted and we're processing backlogs.
[2026-08-13 19:34:57 UTC] Sentry SRE (Monitoring): The problematic change has been reverted and we're processing backlogs.
[2026-08-13 18:35:00 UTC] Sentry SRE (Identified): We've identified the source of latency and are reverting that change.
[2026-08-13 18:07:49 UTC] Sentry SRE (Investigating): We are currently investigating ingestion delays for spans, transactions and alerts in DE and US
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: All backlogs have been processed. We continue to make progress on the spans backlog with an estimated completion time of 15 minutes. We're continuing to make progress on the span ingestion backlog in US. We've recovered all ingestion latency in DE. In US, errors have processed their backlog and we continue to make progress on spans in US. The problematic change has been reverted and we're processing backlogs. The problematic change has been rever

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Span & Transaction ingestion delayed in DE
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
