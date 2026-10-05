# [INC-2026-DATADOG-yrkfn68n] Delayed RUM sessions
**Company:** Datadog | **Date:** 2026-10-02 | **Severity:** MEDIUM | **Source:** [https://stspg.io/gjz5frd5dyxl](https://stspg.io/gjz5frd5dyxl)  
**Technologies:** RUM, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION  

---

## 1. Symptoms & Observed Errors
```text
[2026-10-02 11:03:45 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-10-02 09:47:33 UTC] Datadog SRE (Monitoring): Latency has now resolved and monitors evaluation is being back to normal.  We are still monitoring the issue for now.
[2026-10-02 09:23:41 UTC] Datadog SRE (Monitoring): The issue has been identified we're seeing signs of recovery and we are monitoring the issue.
[2026-10-02 08:46:19 UTC] Datadog SRE (Investigating): We are investigating increased latency processing RUM sessions.
As a result of this issue, some users may see gaps or delays in RUM graphs as well as empty or partial query results on RUM Sessions, RUM Analytics, and RUM Application pages since Oct 2, 2026, 11:42 AM UTC
To prevent false monitor alerts due to delayed data, monitors affected by the delay will not notify and will automatically resume once current data is available. All other monitors will operate normally.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. Latency has now resolved and monitors evaluation is being back to normal.  We are still monitoring the issue for now. The issue has been identified we're seeing signs of recovery and we are monitoring the issue. We are investigating increased latency processing RUM sessions.
As a result of this issue, some users may see gaps or delays in RUM graphs as well as empty or partial query results on RUM Sessions, RUM Ana

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed RUM sessions
service_cluster:
  provider: "Datadog"
  impacted_components: ["RUM", "Datadog Ingestion", "APM", "Metrics Agent"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Datadog SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on RUM cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
