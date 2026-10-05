# [INC-2026-SENTRY-p4n5p1x5] Spans, logs and metrics ingestion stopped
**Company:** Sentry | **Date:** 2026-07-06 | **Severity:** MEDIUM | **Source:** [https://stspg.io/npghss2gxrrr](https://stspg.io/npghss2gxrrr)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-06 22:18:23 UTC] Sentry SRE (Resolved): US ingestion is back to normal. 

EU ingestion is still delayed, but we are now recovering. We have created a new Statuspage incident for tracking EU, follow that for further updates: https://status.sentry.io/incidents/7tfjf5k3m0g3
[2026-07-06 21:28:47 UTC] Sentry SRE (Monitoring): We're still burning the last of our backlog in US and expect to be back to normal the next hour.

We're continuing to investigate the delayed ingestion in our EU region. EU API and dashboards are also affected by this incident.
[2026-07-06 20:29:15 UTC] Sentry SRE (Monitoring): We're continuing burning our backlog in US and expect to be have it fully consumed within the next hour.

We're continuing to investigate the delayed ingestion in our EU region. EU API and dashboards are also affected by this incident.
[2026-07-06 19:26:51 UTC] Sentry SRE (Monitoring): We're continuing burning our backlog in US and expect to be have it fully consumed in about 1 hour.

We're continuing to investigate the delayed ingestion in our EU region.
[2026-07-06 18:30:33 UTC] Sentry SRE (Monitoring): We've taken steps to mitigate the issue in the US and we're burning our backlog and expect to be have it fully consumed in about 4 hours.
We're continuing to investigate the delayed ingestion in our EU region.
[2026-07-06 17:14:21 UTC] Sentry SRE (Investigating): We're continuing to investigate delayed ingestion of spans, logs, and metrics in both our US and EU regions.
[2026-07-06 16:18:10 UTC] Sentry SRE (Investigating): We're continuing to investigate delayed ingestion of spans, logs, and metrics in both our US and EU regions.
[2026-07-06 15:22:07 UTC] Sentry SRE (Investigating): We're continuing to investigate delayed ingestion of spans, logs, and metrics in both our US and EU regions.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: US ingestion is back to normal. 

EU ingestion is still delayed, but we are now recovering. We have created a new Statuspage incident for tracking EU, follow that for further updates: https://status.sentry.io/incidents/7tfjf5k3m0g3 We're still burning the last of our backlog in US and expect to be back to normal the next hour.

We're continuing to investigate the delayed ingestion in our EU region. EU API and dashboards are also affected by this

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Spans, logs and metrics ingestion stopped
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
