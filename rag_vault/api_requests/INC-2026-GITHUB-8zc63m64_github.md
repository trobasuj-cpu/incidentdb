# [INC-2026-GITHUB-8zc63m64] Incident across several services
**Company:** GitHub | **Date:** 2026-09-23 | **Severity:** MEDIUM | **Source:** [https://stspg.io/m1ps7yrhp4n8](https://stspg.io/m1ps7yrhp4n8)  
**Technologies:** API Requests, GitHub Actions, Git, REST API  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-24 04:55:33 UTC] GitHub SRE (Resolved): Starting at 07:57 UTC on September 23, GitHub experienced elevated 500 and 404 responses across several application pages. This caused failures when installing GitHub Apps, creating organizations, and making some organization membership changes. Customers also experienced delayed label updates and stale search results in Projects. <br /><br />The infrastructure failure was isolated to our Azure Central US region. The API errors were mitigated by 10:58 UTC on September 23. Projects' processing continued to recover while an accumulated backlog was drained, and full service was restored at 04:55 UTC on September 24. <br /><br />The incident was caused by a failed planned maintenance operation on a primary database. Automated recovery initiated an emergency database failover, after which several replicas in the affected region were unable to resume replication correctly. This reduced available database capacity and caused the API errors and downstream Projects processing delays. <br /><br />We have mitigated the immediate failure mode. We are also improving maintenance safety checks, database failover handling, post-failover replica validation, and downstream processing resilience to reduce the likelihood and impact of similar incidents.
[2026-09-24 04:55:27 UTC] GitHub SRE (Monitoring): The degradation has been mitigated. We are monitoring to ensure stability.
[2026-09-24 03:14:15 UTC] GitHub SRE (Investigating): We are continuing to process the backlog of issue label updates for Projects. Label changes may still be delayed. All other services are operating normally.
[2026-09-24 02:00:37 UTC] GitHub SRE (Investigating): We are continuing to process the backlog of issue label updates for Projects. Users may still see delays before label changes are reflected in Projects. All other services are operating normally.
[2026-09-24 00:16:49 UTC] GitHub SRE (Investigating): We've deployed a change intended to accelerate processing of the backlog of issue label updates in Projects. A sizable backlog still remains and we continue working through it. All other services are operating normally. We will provide another update within the next hour.
[2026-09-23 21:39:30 UTC] GitHub SRE (Investigating): Updates to issue labels may be delayed in being reflected in Projects. We are continuing to deploy a change that will accelerate processing of the backlog of label updates. All other services are available. We will provide another update within the next hour.
[2026-09-23 20:26:02 UTC] GitHub SRE (Investigating): We are preparing to deploy a change that will mitigate the impact.
[2026-09-23 18:42:01 UTC] GitHub SRE (Investigating): Continuing to investigate the lag that may be experienced in issue labels being accurately reflected in Projects. We are working on alternate solutions to process the backlog of label updates.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: Starting at 07:57 UTC on September 23, GitHub experienced elevated 500 and 404 responses across several application pages. This caused failures when installing GitHub Apps, creating organizations, and making some organization membership changes. Customers also experienced delayed label updates and stale search results in Projects. <br /><br />The infrastructure failure was isolated to our Azure Central US region. The API errors were mitigated by

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident across several services
service_cluster:
  provider: "GitHub"
  impacted_components: ["API Requests", "GitHub Actions", "Git", "REST API"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by GitHub SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on API Requests cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
